Z-Wave Primer
=============

This document covers some important aspects of the Z-Wave
application-level standard that you may come in contact with when
developing device type handlers for Z-Wave devices. If you are already
familiar with Z-Wave development, you can learn how SmartThings
integrates with it in `Building Z-Wave Device
Types <building-z-wave-device-types.html>`__.

Command Classes
---------------

Z-Wave device messages are all called "commands", even if they are just
info reports or other kinds of communications. They are organized into
*command classes* which group related functionality together. Some
devices list which command classes they support in their manuals.

There is a list of the command classes that SmartThings supports here:
`Z-Wave Command Reference <http://build.smartthings.com/zwave.html>`__. Notice
some of them have multiple versions. The Z-Wave standard occasionally
adds a new version of a command class that may add new commands or add
more data fields to existing commands. New versions are
backwards-compatible and generally our command parsing system can handle
different versions interchangeably, but you may need to specify a
specific version in some cases.

Some commonly seen command classes:

-  `**0x20 Basic** <http://build.smartthings.com/zwave.html#basicV1>`__ A
   generalized get/set/report command class that all devices support. It
   is usually mapped to another more specific command class, like Switch
   Binary for switches or Sensor Binary for sensors.
-  `**0x25 Switch
   Binary** <http://build.smartthings.com/zwave.html#switchBinaryV1>`__ Control of
   on/off switches.
-  `**0x26 Switch
   Multilevel** <http://build.smartthings.com/zwave.html#switchMultilevelV3>`__
   Control of dimmer switches.
-  `**0x30 Sensor
   Binary** <http://build.smartthings.com/zwave.html#sensorBinaryV1>`__ Sensors
   with two states, such as motion detectors and open/closed sensors.
-  `**0x31 Sensor
   Multilevel** <http://build.smartthings.com/zwave.html#sensorMultilevelV5>`__
   Sensors that report a numeric value, like temperature or illuminance.
-  `**0x32 Meter** <http://build.smartthings.com/zwave.html#meterV3>`__ Outlets
   and meters that measure energy use.
-  `**0x71
   Alarm/Notification** <http://build.smartthings.com/zwave.html#notificationV3>`__
   The *Alarm* command class was renamed to *Notification* in version 3.
   Used by sensors and other devices to report events.
-  `**0x70
   Configuration** <http://build.smartthings.com/zwave.html#configurationV2>`__
   See *Configuration* section below.
-  `**0x80 Battery** <http://build.smartthings.com/zwave.html#batteryV1>`__
   Battery level reporting for battery powered devices.
-  `**0x84 Wake Up** <http://build.smartthings.com/zwave.html#wakeUpV2>`__ See
   *Listening and Sleepy Devices* section below.
-  `**0x85
   Association** <http://build.smartthings.com/zwave.html#associationV2>`__ See
   *Association* section below.
-  `**0x86 Version** <http://build.smartthings.com/zwave.html#versionV1>`__ All
   devices report their Z-Wave framework and firmware version on
   request.
-  `**0x72 Manufacturer
   Specific** <http://build.smartthings.com/zwave.html#manufacturerSpecificV2>`__
   All devices report their manufacturer and model (via numeric code).
-  `**0x98 Security** <http://build.smartthings.com/zwave.html#securityV1>`__
   Commands to and from security-sensitive devices can be sent encrypted
   by wrapping them in *SecurityMessageEncapsulation* commands.
-  `**0x60
   Multi-Channel/Multi-Instance** <http://build.smartthings.com/zwave.html#multiChannelV3>`__
   The *Multi Instance* command class was renamed to *Multi Channel* in
   version 3. It is used by devices to distinguish between multiple
   control or reporting end points.

Listening and Sleepy Devices
----------------------------

Z-Wave devices that are plugged in to power are called ***listening***
devices because they keep their receiver on all the time. Listening
devices act as repeaters and therefore extend the Z-Wave mesh network.

Battery powered Z-Wave devices such as sensors or remote controllers are
***sleepy*** – they turn off their receivers to save energy, so you
can't send them commands at any time. Instead, they wake up at a regular
interval and send a
`WakeUpNotification <http://build.smartthings.com/zwave.html#wakeUpV2/wakeUpNotification>`__
to alert other devices that they will be listening for incoming commands
for the next few seconds. The
`WakeUpIntervalSet <http://build.smartthings.com/zwave.html#wakeUpV2/wakeUpIntervalSet>`__
command is used to configure both how often the device will wake up and
which controller it will send its *WakeUpNotification* to. When the
controller gets the *WakeUpNotification* and has no commands to send to
the device, it can send
`WakeUpNoMoreInformation <http://build.smartthings.com/zwave.html#wakeUpV2/wakeUpNoMoreInformation>`__
to tell the device that it can go back to sleep.

Some battery powered devices like door locks and thermostats have to be
able to receive commands at any time. These are known as ***beamable***
devices, because they wake up for only a tiny slice of time each second
or quarter-second and listen for a "beam". Thus, the sending device must
"beam" the receiving device for a full second to wake it up fully before
sending a command. This makes communication with these devices take a
significantly longer time than with a normal listening device.

Configuration
-------------

A Z-Wave device can use the
`Configuration <http://build.smartthings.com/zwave.html#configurationV2>`__
command class to allow the user to change its settings. Configuration
parameters and their interpretation vary between device models, and are
usually detailed in the device's manual or technical documentation.

The command class includes commands to read and set configuration
parameter values. One thing to be careful of is that the
`ConfigurationSet <http://build.smartthings.com/zwave.html#configurationV2/configurationSet>`__
command encodes the setting value in a 1, 2, or 4 byte format, and many
devices will only properly interpret the value if it is sent in the same
byte format. When sending a *ConfigurationSet*, make sure to set the
'size' argument to the same value as it has in an incoming
`ConfigurationReport <http://build.smartthings.com/zwave.html#configurationV2/configurationReport>`__
from the device for the parameter number in question.

Association
-----------

The `Association <http://build.smartthings.com/zwave.html#associationV2>`__
command class is used to tell a Z-Wave device that it should send
updates to another device. It provides the ability to add associated
devices to different numbered groups that can have different meanings.
This functionality is used in a few different ways, often detailed in
the device's manual or technical documentation.

-  Some sensors will send reports of the events they detect only to
   devices that have been added to a specific association group.
-  Many sensors will send
   `BasicSet <http://build.smartthings.com/zwave.html#basicV1/basicSet>`__
   commands to associated devices, for example to turn a light on when a
   door opens and off when it closes.
-  Some devices have multiple groups for different uses, like group 1
   gets sent *BasicSet* commands, group 2 gets sent *SensorBinaryReport*
   events, and group 3 gets sent *BatteryReport* updates.
-  Most door locks will send status updates to associated devices when
   they are locked or unlocked manually.

The SmartThings hub automatically adds itself to association group 1
when a device that supports association joins the network. If this is
inappropriate for your device type, your device type handler can use
`AssociationRemove <http://build.smartthings.com/zwave.html#associationV2/associationRemove>`__
to undo it. To associate to a group higher than 1, the device type
handler can send
`AssociationSet <http://build.smartthings.com/zwave.html#associationV2/associationSet>`__.
The hub's node ID is provided to device type handler code in the
variable ``zwaveHubNodeId``.

`Z Wave Device Example ➞ <z-wave-device-reference.html>`__