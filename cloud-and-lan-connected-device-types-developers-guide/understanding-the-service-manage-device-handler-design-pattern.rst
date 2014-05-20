Understanding the Service-Manager/Device-Handler Design Pattern
===============================================================

Basic Overview
--------------

Devices that connect through the internet as a whole (cloud) or LAN
devices (on your local network) require a defined Service-Manager
SmartApp, in addition to the usually expected Device-Handler. The
service manager makes the connection with the device, handling the input
and output interactions, and the device handler parses messages, as
usual.

LAN Connected Devices
---------------------

When using a LAN connected device, the service manager is used to
discover and initiate a connection between the device and your hub,
using the protocols SSDP or mDNS/DNS-SD. Then the device-handler uses
UPnP/SOAP Calls or REST Calls to communicate outgoing messages between
the hub and device.

Cloud-Connected Devices
-----------------------

When using a cloud-connected device, the service manager is used to
discover and initiate a connection between the device and your hub,
using OAuth connections to external third parties. Then the
device-handler uses this connection to communicate between the hub and
device.
