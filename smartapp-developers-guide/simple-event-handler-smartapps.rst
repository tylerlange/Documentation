Event-Handler SmartApps
=======================

What Are Events?
----------------

Technically speaking, events are a report of a change in or update to
one of a device’s attributes. For you, that means a door opening, the
temperature changing, sound detected or countless other happenings in
the physical graph.

Subscribing to Events
---------------------

There are plenty of events going off throughout your physical graph, but
you don't need to know about all of them. You need to specifically
subscribe to the ones you want.

To subscribe, you define what device you'd like to observe, what event
you'd to listen for, and a handler method to call when that event fires
(indicating an attribute has changed).

::

    subscribe(contact1,"contact",contactHandler)

    contactHandler(evt){

    }

You can also subscribe to specific states of events. For example, if you
only want to subscribe to events for a door opening but not closing, you
could subscribe to just those events.

::

    subscribe(contact1,"contact.open",contactOpenHandler)

    contactOpenHandler(evt){

    }

.. ADD Link

Events versus Current Value
---------------------------

When you subscribe to an event, your handler will be passed the event
for you to read the current attribute value.

There are often cases, however, where you'd like to observe a different
device's attributes within your handler method.

You can query attributes of any devices defined in your preferences.
When querying attributes, you can either request just the current value
for the attribute or the event as a whole.

**String latestValue(String attribute)** 

Returns the latest value for a specific attribute of the device. For
example, if you wanted to know whether a switch was on or off.

::

    def myHandler(evt) {
        def latestValue = device1.latestValue("attributeName")
    }

**String latestState(String attribute)** 

Returns the most recent event for the specified attribute, which
provides you meta information along with the latest value. For example,
if you wanted to know when a switch was turned on or off.

::

    def foo(evt) {
        def latestState = device1.latestState("attributeName")
        def latestStateDate = latestState.dateCreated
    }

Invoking Commands on a Device
-----------------------------

Let's make something happen. When you want to interface with a device,
you can invoke commands on it. For example, you might want your SmartApp
to turn on a light.

::

    switch.on()

That's it. The light will now turn on.

Every capability contains a list of relevant commands. Unless you define
custom commands, you are limited to running only commands that fall
within your devices cumulative list of relevant commands, which is built
dependent on your capabilities.

Next Article: `Dashboard Solution SmartApps
➞ <dashboard-solution-smartapps.md>`__
