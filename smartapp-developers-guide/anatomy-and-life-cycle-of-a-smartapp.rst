Anatomy & Life-Cycle of a SmartApp
==================================

SmartApps are applications created by SmartThings and the SmartThings
community as a whole. They allow a user to tap into the capabilities of
their devices to automate their lives, and can be installed via the
SmartThings mobile client applications.

Types of SmartApps
------------------

**Event-Handler SmartApps**

Event Handler SmartApps are the most common apps developed by our
community. They allow you to subscribe to events from devices and call a
handler method upon their firing. This method can then do a variety of
things, most commonly invoking a command on another device. We're
confident that if you are familiar with back end development of web
sites, then you will be more than capable of developing SmartApps.

A very simple example of a SmartApp would involve you walking through a
door and having the lights turn on automatically.

**Solution Module SmartApps**

These apps exist within the dashboard of the SmartThings app interface,
and are containers for other SmartApps. The idea behind Solution Module
SmartApps is to combine SmartApps that, in the real world, intuitively
go together. One example of this would be the "Home & Family" section of
the dashboard which allows you to see the comings and goings of your
family.

Solution Module SmartApps have traditionally been built by our internal
team, but we will be opening them up for external development in the
near future.

**Service Manager SmartApps**

Service Manager SmartApps are used to connect to LAN or cloud devices,
such as the Sonos or WeMo. They are the connecting glue between the
unique protocols of your external devices and a device type handler
you'd create for those devices. They discover devices and then continue
to maintain the connection for those devices.

The Service Manager SmartApp must be installed when a user utilizes a
device using LAN or the cloud, so for example, there is a Sonos Service
Manager SmartApp that is installed when pairing with a Sonos.

Installation & Configuration
----------------------------

The enduser installs your SmartApp through the SmartThings Application.
Upon installation, they will go through a process of configuring
settings for their unique instance of the SmartApp.

Installed Method
----------------

A method that is called when the app is first installed

This is where you would subscribe to the events coming from the devices
you described in the preferences() method.

::

    def installed() {}

Subscriptions
-------------

Subscriptions allow you to listen for particular events. Upon the event
firing, a variable is set to the event and a handler method is called.

.. TODO add link

::

    subscribe(motion1, "motion", motionHandler)

You can also subscribe to specific event stats like "switch.on" or
"motion.active". This is useful when you only want to subscribe to some
events.

::

    subscribe(motion1, "motion.active", motionActiveHandler)

SmartApp Execution
------------------

SmartApps exist transiently. They are instantiated, run though logic,
execute commands, and terminate. They aren't always running, and
therefore SmartApps must be told to run.

SmartApps can be triggered and executed by

1. **Event Subscription:** An attribute changes on a device, which
   creates an event, which triggers a subscription, which calls a
   handler method within your SmartApp.
2. **Scheduled Events:** Using a method like runIn(), you call
   a method within your SmartApp at a particular time .
3. **Endpoint Triggers:** Using our `web services
   API <../smartapp-web-services-developers-guide/overview.md>`__, you
   create an endpoint accessible over the web that calls a method within
   your SmartApp.

SmartApp Sandboxing
-------------------

SmartApps are developed in a sandboxed environment. The sandbox is a way
to limit developers to a specific subset of the Groovy language for
performance and security. We `have
documented <smartthings-sandbox-groovy-limitations.md>`__ the main ways
this should affect you.

Updated Method
--------------

A method that is called when an already installed app is updated by the
user

Sometimes the app is updated by the user, for example they could change
which device the app is pointing to. In this case then you need to
unsubscribe from the previous device's events and subscribe to the new
device's instead.

::

    def updated()
    {
      unsubscribe()
      subscribe(motion1, "motion", motionHandler)
    }

Uninstalled Method
------------------

A method that is called when the user uninstalls an app. You don't need
to do housekeeping of your application as this point, as the SmartThings
platform will automatically clean up all extraneous data within the
platform before it's deleted. This method is rarely utilized.

::

    def uninstalled()
    {
      doSomething()
    }

