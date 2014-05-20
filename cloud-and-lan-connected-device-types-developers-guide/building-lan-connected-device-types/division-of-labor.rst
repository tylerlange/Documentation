LAN-Connected Device Types: Division of Labor
=============================================

Service-Manager Responsibilities
--------------------------------

The service manager is responsible for the discovery of the devices. It
sends out a request and parses through the response, finding just the
devices you are looking for. Upon discovery, it allows you to add
device(s) that is has found. From there, it saves your connection to be
able to make future interactions with the device.

Device-Type Responsibilities
----------------------------

The device type is responsible for creating and receiving device
specific messages, and allowing them to work within the SmartThings
infrastructure. It takes in a SmartApp specific command and outputs
device specific commands. It also allows you to subscribe to responses
from the device and trigger other commands as needed.
