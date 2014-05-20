Standard Device Type Pattern
============================

Finger Printing
---------------

Devices attached to the SmartThings platform are always of a specific
type. We a new device is joined, it is recognized through a
fingerprinting process and assigned the appropriate type. That process
takes into account:

-  Protocol (e.g. Zigbee, Z-Wave, UPnP, etc)
-  Manufacturer
-  Model Number
-  Protocol-Specific Functions (Zigbee Clusters, Z-Wave Command Classes,
   UPnP Services)

The fingerprinting process is designed to quickly type the device so
that we know what software to use to integrate the device into the
SmartThings platform. The modular software that accomplishes that
integration is known as a “Device-Type Handler”. Device-Type Handlers
are software modules that “handle” the protocol-specific aspects of the
device on one side and translate those to and from the generalized
capabilities and attributes that are exposed within the SmartThings
platform.

Parsing Incoming Messages
-------------------------

Messages come in from a third party device with unique syntax. Whether
they are a ZigBee, ZWave, or LAN device, the messages will look
different, so they need to be parsed in such a way that the SmartThings
system knows what to do with them. The Device Type must do the heavy
lifting of converting the messages, so as to abstract SmartApps from
device specific syntax.

Generating Events
-----------------

Once a message is parsed, it can be determined what the device is
looking to accomplish. An event is triggered based on the parsed
message, so for example, the message might specify that a switch has
been turned on, so an on event would be fired.

Command Methods
---------------

You also have the capability within SmartThings to invoke a command on a
device. You could call an on method on a switch, to turn it on. Every
command for a device should have a method mapped to it within your
device type.

Formatting Outbound Commands
----------------------------

When a command method is called, it's job is to convert the SmartThings
command into a readable format for the end device. One example would be
calling an on method, which would then format a ZigBee command for on.
