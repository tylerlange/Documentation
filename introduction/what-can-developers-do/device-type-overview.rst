Device Type Overview
====================

The SmartThings architecture provides a unique abstraction of devices
from their distinct capabilities and attributes in a way that allows
developers to build applications that are insulated from the specifics
of which device they are using. For example, there are lots of
wirelessly controllable “switches”. Switches can come in the form of
pluggable outlet overlays, in-wall outlets, in-wall light switches, etc.
At it's core, however, a switch is any device that can be turned On or
Off.

When a SmartApp interacts with the virtual representation of a device,
it knows that the device supports certain actions based on it’s
capabilities. A device that has the ‘switch’ capability must support
both the ‘on’ and ‘off’ actions. In this way, all switches are the same,
and it doesn’t matter to the SmartApp what kind of switch is actually
involved. This layer of abstraction is key to the successful function
and flexibility of the SmartThings platform, and is made possible by a
concept called ‘device-type handlers’. Architecturally, device-type
handlers are the bridge between generic capabilities and the device or
protocol specific interface actually used to communicate with the
device.

Using device type handlers, you can interface with the following types
of devices:

Hub Connected Devices
---------------------

**ZigBee Home Automation Devices**

On the SmartThings platform we support devices connected via the ZigBee
Home Automation standard.

ZigBee is a Personal Area Mesh Networking standard for connecting and
controlling devices. A mesh network transfers information between
distributed devices, allowing it to have an extended range. It offers
limited interference, as it's able to use multiple paths to get to the
same point. A point to point network would have a single path and have
more single points of failure.

ZigBee is an open standard supported by the ZigBee Alliance. For more
information on Zigbee see http://en.wikipedia.org/wiki/ZigBee. Because
it's open, there are a variety of vendors engaged in providing ZigBee
based products.

It's easy to connect your ZigBee devices to SmartThings with a custom
device type.

Other Helpful Links

| `ZigBee Home Automation
Overview <http://www.zigbee.org/Standards/ZigBeeHomeAutomation/Overview.aspx>`__
*(zigbee.org)*
| `ZigBee Home Automation
Products <http://www.zigbee.org/Products/ByStandard/ZigBeeHomeAutomation.aspx>`__
*(zigbee.org)*

**Z-Wave Devices**

On the SmartThings platform we also support devices connected via the
Z-Wave specification. Z-Wave is a low-power wireless mesh networking
protocol for home automation and lighting control. There are many Z-Wave
devices available on the market, almost all of which are possible to
integrate with SmartThings by building custom device types.

For more information on Z-Wave see http://en.wikipedia.org/wiki/Z-Wave.

Other Helpful Links

| `Z-Wave Alliance <http://www.z-wavealliance.org/>`__
| `Z-Wave Products <http://products.z-wavealliance.org/>`__

Cloud Connected Devices
-----------------------

Cloud-connected devices are devices that are managed by a third-party
cloud service, whether it's operated by the device manufacturer (e.g,
Quirky Wink, Ecobee thermostat) or a third-party aggregator (e.g.,
Xively, ThingWorx).

SmartThings support of cloud-connected devices is limited to those whose
cloud services authenticate through a standard OAuth2 flow, and
communicate via HTTP-based APIs. While it is technically possible to
store and encypt the user's credentials for the remote service rather
than completing an OAuth2 flow and storing only an access token, it is
highly discouraged.

LAN-Connected Devices
---------------------

On the SmartThings platform we also support devices connected via a LAN
(Local Area Network). These devices are typically connected to your
network through WiFi. As the SmartHub connects to your network directly
through the ethernet cable, we are able to interact with these devices
on a network level.

You can communicate with LAN-Connected Devices through our internal
library via REST or with SOAP requests using UPnP (Universal Plug and
Play).
