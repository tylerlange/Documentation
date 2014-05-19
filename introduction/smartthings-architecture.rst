Smart Things Architecture
=========================

.. figure:: ../img/architecture/overview.png
   :alt: Container Hierarchy

   Container Hierarchy
Devices
-------

Devices are the building blocks of the SmartThings infrastructure.
There's a huge variety in the devices you can use, some created by
SmartThings and some not. Here's a breakdown:

| **SmartSense Motion**
| The SmartSense Motion detector is an IR-based motion detector that
triggers an event when human or large animal motion is detected in its
field of vision. When plugged in (via microUSB), the SmartSense Motion
detector also acts as a repeater to extend the range of the ZigBee
network of connected Things. On battery power, anticipated life of
operation is up to 2 years.

| **SmartSense Multi**
| SmartSense Multi is a multipurpose Thing that reports movement,
vibration, orientation and temperature data to the SmartHub. Based upon
the SmartApp installed, the SmartSense Multi will report the desired
information.

| **SmartSense Presence**
| The SmartSense Presence tag is small device you can put on your
keychain, a backpack or your pet's collar to report presence (nearby or
not) to the SmartThings

| **Third Party Products**
| In addition to our own devices, and through our compatibility with
standards such as Zigbee, ZWave, and IP/WiFi, we are compatible with
literally hundreds of offtheshelf thirdparty devices.

Hub
---

| The SmartThings Ethernet Hub connects directly to your broadband
router and provides communication between all connected Things and the
SmartThings cloud and mobile application.
| + Connects any SmartThings or SmartThings Ready device to your
SmartThings account.
| + Simply plug into your Ethernet router and provide power.
| + Build your own SmartThings kit by combining with other SmartThings
devices.
| + Install SmartApps to change your notifications and events directly
from your mobile phone.
| + Also works with standard ZigBee and ZWavedevices, such as GE ZWave
inwall switches and outlets.

Connectivity Management
-----------------------

Connectivity Management is the layer that connects your SmartThings hub
and client devices (mobile phones) to our servers, and the cloud as a
whole. We have two parts of this layer currently:

-  Hub Connectivity (DevCon) connects your hub to the cloud.
-  Client Connectivity (ClientCon) connects your client devices to the
   cloud.

These are the highways by which your messages are sent to the internet.

Device-Type Execution
---------------------

This layer includes Device Type Handlers and all of the core code that
helps them run. The SmartThings system will determine what device type
you are using for the incoming messages, and use that particular device
type to parse your messages. The input of the device type handler are
device specific messages, and the output is normalized SmartThings
events. Note that one message, can lead to many SmartThings events.

Subscription Management
-----------------------

The events that are triggered by the device type handler layer are
matched up with what SmartApp is using them.

SmartApp Execution
------------------

The SmartApp is run, driven by subscriptions firing and any other
preferences or conditions.

Core APIs
---------

SmartThings provides APIs that can be used both internally and
externally to power experiences around our platform.

The Core API drives the interactions between our mobile applications and
SmartThings. Please contact us if you are interested in utilizing the
Core API to build custom solutions.

SmartThings also has an Event Streaming API that allows us to keep up
with our devices as they are being utilized in real time.

Web-UI & IDE
------------

The Web-UI sits on top of all of the other technology and allows you to
monitor your devices and hubs. You can also write code within the IDE
for SmartApps and Device Types. What you do here filters down through
the rest of our system.

