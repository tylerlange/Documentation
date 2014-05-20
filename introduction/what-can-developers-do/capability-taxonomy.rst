Capability Taxonomy
===================

`Capabilities <https://graph.api.smartthings.com/ide/doc/capabilities>`__
represent the common taxonomy that allows us to link SmartApps with
Device Types. An application interacts with devices based on their
capabilities, so once we understand the capabilities that are needed by
a SmartApp, and the capabilities that are provided by a device, we can
understand which devices (based on their device type and inherent
capabilities) are eligible for use within a specific SmartApp.

The `capability
taxonomy <https://graph.api.smartthings.com/ide/doc/capabilities>`__ is
evolving and is heavily influenced by existing standards like Zigbee,
Z-Wave, UPnP, and many others.

`Capabilities <https://graph.api.smartthings.com/ide/doc/capabilities>`__
themselves may decompose into both ‘Actions’ or ‘Commands’ (these are
synonymous), and Attributes. Actions represent ways in which you can
control or actuate the device, whereas Attributes represent state
information or properties of the device.

Attributes & Events
-------------------

Attributes represent the various properties or characteristics of a
device. Generally speaking device attributes represent a current device
state of some kind. For a temperature sensor, for example, ‘temperature’
might be an attribute. For a door lock, an attribute such as ‘status’
with values of ‘open’ or ‘closed’ might be a typical.

Commands
--------

Commands are ways in which you can control the device. A capability is
supported by a specific set of commands. For example, the ‘Switch’
capability has two required commands: ‘On’ and ‘Off’. When a device
supports a specific capability, it must generally support all of the
commands required of that capability.

Custom Capabilities
-------------------

If you are integrating or building a device that requires a capability
that isn’t listed here, no problem! You can add a custom capability to
the platform yourself, and we will then look for common threads across
new custom capabilities in order to determine which ones should be made
part of the standard taxonomy.
