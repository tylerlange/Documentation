State
=====

What is SmartApp State?
-----------------------

State is used to store arbitrary data for the SmartApp between events
and over time. Because SmartApps aren't always running, you need state
to hold your persistent data.

| If you want to save a variable that will be persisted between events,
you must use the application state. You can assign Strings, Numbers,
Lists, Maps, Booleans, etc. to arbitrary key names in the form:
|  state.key = value

As far as practical applications go, you might use state to store the
last time something was run, or perhaps history on how you've messaged
to the enduser. Use it for anything that needs to persist and won't
already be saved by your preferences.

Using SmartApp State
--------------------

To use state, simply use the predefined object **state**, and define any
properties you'd like to use. They will then be accessible anywhere
within your SmartApp.

::

    def installed()
    {
      state.count = 0
      state.myMap = [foo:"bar"]
      state.name = "SmartThings"
    }

    def event(evt) {
      log.debug "Foo is: ${state.myMap.foo}" //Will print out "Foo is: bar"
      log.debug "Name is: ${state.name}" //Will print out "Name is: SmartThings"
      state.count = state.count + 1
      log.debug "Count is: ${state.count}" //Will print out "Count is: 1", and will increase every time event gets called
    }

State Limitations
-----------------

The main limitation of SmartApp state is that, because we use JSON to
store state, your data types might not persist after being saved and
returned. Strings and numbers should have no issues, but be aware of
this, in particular when it comes to storing dates. This would be one
way to deal with parsing a date string after it comes back from state.

::

    it.sentDate.toSystemDate()

You could also just store the time in milliseconds using an epoch
timestamp.

Next Article: `SmartThings Sandbox Groovy Limitations
➞ <smartthings-sandbox-groovy-limitations.md>`__
