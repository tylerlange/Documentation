Cloud-Connected Device Types: Division of Labor
===============================================

Service-Manager Responsibilities
--------------------------------

The service manager is responsible for the discovery of the devices. It
sends out a request to a third party cloud and parses through the
response, finding just the devices you are looking for. Upon discovery,
it allows you to add device(s) that it has found. From there, it saves
your connection to be able to make future interactions with the device.

Device-Type Responsibilities
----------------------------

The device type is responsible for creating and receiving device
specific messages, and allowing them to work within the SmartThings
infrastructure. It takes in a SmartApp specific command and outputs
device specific commands to be passed to the cloud. It also allows you
to subscribe to responses from the device and trigger other commands as
needed.
