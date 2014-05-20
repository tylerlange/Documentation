LAN-Connected Device Types: Building the Service Manager
========================================================

Discovery
---------

**SSDP**

`SSDP <http://en.wikipedia.org/wiki/Simple_Service_Discovery_Protocol>`__
is the main protocol used to find devices on your network. It serves as
the backbone of `Universal Plug and
Play <http://en.wikipedia.org/wiki/Universal_Plug_and_Play>`__, which
allows you easily connect new network devices to a system. To discovery
new devices, you'd use something like this:

::

    sendHubCommand(new physicalgraph.device.HubAction("lan discovery urn:schemas-upnp-org:device:ZonePlayer:1", physicalgraph.device.Protocol.LAN))

This is an example of discovering devices using the LAN protocol. The
main message to be sent through the hub is

::

    lan discovery urn:schemas-upnp-org:device:ZonePlayer:1

The protocol is **lan**. The unique resource name (URN) is built with
the Namespace Identifier (NID) **schemas-upnp-org** and Namespace
Specific String (NSS) **device:ZonePlayer:1** according to the `URN
Syntax Guide <http://www.ietf.org/rfc/rfc2141.txt>`__.

For sonos, the device specific search term is **ZonePlayer:1**, but that
will change per device. The search term for a particular device using
UPnP should be published on documentation for the device, but you may
also have to contact the manufacturer directly. The above command is
typically called in a timing loop.

**mDNS/DNS-SD**

mDNS/DNS-SD is another popular protocol used to find devices on a
network. It's made up of Multicast DNS and DNS-based service discovery.
Known as Bonjur in the Apple ecosytem, Applie relies on mMDNS/DNS-SD for
services such as iChat or AppleTV. **Our support for mDNS/DNS-SD isn't
quite ready yet, but will be released on a future hub firmware
upgrade.**

*How it Works*

1. The device generates a unique IP address and calls out to the network
   to confirm the IP is not in use.
2. If the IP is not in use, it then passes a unique name to the network
   to confirm the name is not in use.
3. If the name not in use, the device starts a service.
4. The name is published as a record on the network, so others can find
   it.
5. The client searches for a particular device by name, and finds the
   device.

As a device type developer, you are responsible only for step 5 in the
process.

More information on Bonjur can be found in `Apple's Developer
documentation <https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/NetServices/Articles/NetServicesArchitecture.html#//apple_ref/doc/uid/20001074-SW1>`__.

Your discovery request would look like this:

::

    sendHubCommand(new physicalgraph.device.HubAction("lan discover mdns/dns-sd ._smartthings._tcp._site", physicalgraph.device.Protocol.LAN))

The main message to be sent through the hub is

::

    lan discover mdns/dns-sd ._smartthings._tcp._site

The protocol is **lan**, it's sent through **mdns/dns-sd** and the
service's record name is \*\*.\ *smartthings.*\ tcp.\_site\*\*.

Handling Updates (Adds/Changes/Deletes)
---------------------------------------

When there are changes within the scope of your devices, the service
manager should handle those updates.

**Adding Devices**

A subscription is created to listen for a location event. The way the
system is currently setup, we can't listen specifically for discovery
events, but rather we listen for any location events, indicating a
device has been added. Upon the event firing, a handler is called, in
this case **locationHandler**.

::

    if(!state.subscribe) {
      log.debug "subscribe to location"
      subscribe(location, null, locationHandler, [filterEvents:false])
      state.subscribe = true
    }

And then later the locationHandler method is defined which adds the
device to a collection of devices. Note that because we aren't just
listening for discovery events, we have to parse the response to
properly determine if a discovery has been made.

Within the LocationHander, you need to see if the device is currently
part of the devices collection in your state. You can check this via any
unique identifier of your device. If it's not already registered in your
state, go ahead and add it.

::

    def locationHandler(evt) {
      def description = evt.description
      def hub = evt?.hubId

      def parsedEvent = parseEventMessage(description)
      parsedEvent << ["hub":hub]

      if (parsedEvent?.ssdpTerm?.contains("urn:schemas-upnp-org:device:DeviceIdentifier:1"))
      {

        def devices = getDevices()

        if (!(devices."${parsedEvent.ssdpUSN.toString()}"))
        {
          devices << ["${parsedEvent.ssdpUSN.toString()}":parsedEvent]
        }

    def getDevices()
    {
      if (!state.devices) { state.devices = [:] }
      state.devices
    }

The example above uses SSDP, you could also use mDNS/DNS-SD. You just
need to change what attributes are being used. For example, you could
replace this:

::

    if (parsedEvent?.ssdpTerm?.contains("urn:schemas-upnp-org:device:DeviceIdentifier:1"))

with this:

::

    if(parsedEvent?.mdnsPath)

and this:

::

    if (!(devices."${parsedEvent.ssdpUSN.toString()}"))

with this:

::

    if (!(devices."${parsedEvent?.mac?.toString()}"))

**Changing Devices**

You need to monitor your devices networking information for changes. By
using a unique identifier within your device, you can check that IP and
port information hasn't changed.

Using SSDP:

::

    if ((devices."${parsedEvent.ssdpUSN.toString()}")){
      def d = devices."${parsedEvent.ssdpUSN.toString()}"
      boolean deviceChangedValues = false

        if(d.ip != parsedEvent.ip || d.port != parsedEvent.port) {
            d.ip = parsedEvent.ip
            d.port = parsedEvent.port
            deviceChangedValues = true
        }
    }

Using mDNS/DNS-SD:

::

    if ((devices."${parsedEvent?.mac?.toString()}")) {
      def d = device."${parsedEvent.mac.toString()}"
      boolean deviceChangedValues = false

      if(d.ip != parsedEvent.ip || d.port != parsedEvent.port) {
          d.ip = parsedEvent.ip
          d.port = parsedEvent.port
          deviceChangedValues = true
      }
    }

If values did change, then you need to manually update your devices
within the SmartApp.

::

    if (deviceChangedValues) {
                def children = getChildDevices()
                children.each {
                    if (it.getDeviceDataByName("mac") == parsedEvent.mac) {
                        it.setDeviceNetworkId((parsedEvent.ip + ":" + parsedEvent.port)) //could error if device with same dni already exists
                    }
                }
        }

**Deleting Devices**

You don't need to handle deleting devices within the Service Manager.
Devices, by nature, can become connected or disconnected at various
times, and we still want them to persist. An example of this would be a
laptop - if you were to take it with you somewhere, you'd still want it
to pair properly later.

The enduser will need to manually delete their device within the
SmartThings application.

Creating Child Devices
----------------------

After you have discovered all your devices and the app has been
installed, you need to add the device(s) the user has selected as a
child device. You will iterate through a collection created from the
user's input, and find just the devices they picked and add them.

::

    selectedDevices.each { dni ->
        def d = getChildDevice(dni)
        if(!d) {
            def newDevice = devices.find { (it.value.ip + ":" + it.value.port) == dni               }
            d = addChildDevice("smartthings", "Device Name", dni, newDevice?.value.hub, ["label":newDevice?.value.name])
            subscribeAll() //helper method to update devices
        }
    }
