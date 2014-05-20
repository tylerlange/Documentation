SmartApp Overview
=================

Event Handler SmartApps
-----------------------

Event Handler SmartApps are the most common apps developed by our
community. They allow you to subscribe to events from devices and call a
handler method upon their firing. This method can then do a variety of
things, most commonly invoking a command on another device. We're
confident that if you are familiar with back end development of web
sites, then you will be more than capable of developing SmartApps.

A very simple example of a SmartApp would involve you walking through a
door and having the lights turn on automatically.

Solution Module SmartApps
-------------------------

These apps exist within the dashboard of the SmartThings app interface,
and are containers for other SmartApps. The idea behind Solution Module
SmartApps is to combine SmartApps that, in the real world, intuitively
go together. One example of this would be the "Home & Family" section of
the dashboard which allows you to see the comings and goings of your
family.

Solution Module SmartApps have traditionally been built by our internal
team, but we will be opening them up for external development in the
near future.

Service Manager SmartApps
-------------------------

Service Manager SmartApps are used to connect to LAN or cloud devices,
such as the Sonos or WeMo. They are the connecting glue between the
unique protocols of your external devices and a device type handler
you'd create for those devices. They discover devices and then continue
to maintain the connection for those devices.

The Service Manager SmartApp must be installed when a user utilizes a
device using LAN or the cloud, so for example, there is a Sonos Service
Manager SmartApp that is installed when pairing with a Sonos.
