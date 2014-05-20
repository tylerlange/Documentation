LAN-Connected Device Types: Building the Device Type
====================================================

Making Outbound HTTP Calls
--------------------------

Depending on the type of device you are using, you will send requests to
your devices through the hub via REST or UPnP.

**REST Calls**

After making the initial connection you can use standard
`REST <http://en.wikipedia.org/wiki/Representational_state_transfer>`__
calls to communicate with the device. There is a helper method for
creating a REST call via the supplied parameters. An example of the
syntax within the SmartThings platform is as follows:

::

    def result = new physicalgraph.device.HubAction(
            method: "GET",
            path: path,
            headers: [HOST:host]
    )

**UPnP/SOAP Calls**

Alternatively, after making the initial connection you can use UPnP.
UPnP uses `SOAP <http://en.wikipedia.org/wiki/SOAP_%28protocol%29>`__
(Simple Object Access Protocol) messages to communicate with the device.
There is a helper method for creating a SOAP message via the supplied
parameters. An example of the syntax within the SmartThings platform is
as follows:

::

    def result = new physicalgraph.device.HubSoapAction(
        path:    path,
        urn:     "urn:schemas-upnp-org:service:$service:1",
        action:  action,
        body:    body,
        headers: [Host:host, CONNECTION: "close"]
    )

Typically the above SOAP call is defined within a method, so that you
could then make requests for the end device to handle like this.

::

    deviceAction("Go")

*Registering for Callbacks*

If you'd like to hear back from a LAN connected device upon a particular
event, you need to register for a callback. You can subscribe to an
event of the device, passing the device your hubs IP address as well as
a path to callback to. The syntax for this using uPnP is as follows:

::

    subscribeAction("/path/of/event")

    private subscribeAction(path, callbackPath="") {
        def address = device.hub.getDataValue("localIP") + ":" + device.hub.getDataValue("localSrvPortTCP")
        def parts = device.deviceNetworkId.split(":")
        def ip = convertHexToIP(parts[0])
        def port = convertHexToInt(parts[1])
        ip = ip + ":" + port

        def result = new physicalgraph.device.HubAction(
            method: "SUBSCRIBE",
            path: path,
            headers: [
                HOST: ip,
                CALLBACK: "<http://${address}/notify$callbackPath>",
                NT: "upnp:event",
                TIMEOUT: "Second-3600"])
        result
    }

Inbound HTTP Requests
---------------------

**End-Point Mapping**

In order to receive callback events back from your device, you need to
map an endpoint within your device type handler. You do this by defining
a path and then which handler method should be called depending on the
action used.

::

    mappings {
      path("/sample") {
        action: [
          GET: "listMethod",
          PUT: "updateMethod"
        ]
      }
    }

**Handler Methods**

Once the endpoint has been called, it triggers the handler method
specified. In this case there would be two.

::

    listMethod(){}
    updateMethod(){}
