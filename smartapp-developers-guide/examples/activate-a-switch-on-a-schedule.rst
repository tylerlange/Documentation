Activate A Switch On A Schedule
-------------------------------

::

    /**
     *  Activate A Switch On A Schedule
     *
     *  Author: SmartThings
     */
    preferences {
        section("Turn on/off a switch...") {
            input "switch1", "capability.switch"
        }
        section("At what time?") {
            input "cron1", "cron", title: "When?"
        }
    }

    def installed() {
        log.debug "Installed with settings: ${settings}"
        schedule(cron1, "scheduleCheck")
    }

    def updated() {
        log.debug "Updated with settings: ${settings}"
        unschedule()
        schedule(cron1, "scheduleCheck")
    }

    def scheduleCheck()
    {
        log.debug "scheduledCheck: $settings"
        def latestValue = switch1.currentSwitch

        if(latestValue == "off") {
            log.debug "Switch is currently off, switching on"
            switch1.on()
        } else if(latestValue == "on") {
            log.debug "Switch is currently on, switching off"
            switch1.off()
        } else {
            log.debug "Latest value is not on or off, by default turn switch off. Value: $latestValue"
            switch1.off()
        }
    }

This SmartApp takes in a switch and a particular time as an input, and
turns a switch on or off at that specified time. Let's break it down.

::

    preferences {
        section("Turn on/off a switch...") {
            input "switch1", "capability.switch"
        }
        section("At what time?") {
            input "cron1", "cron", title: "When?"
        }
    }

The preferences section sets up the experience for the end user to
select their unique options.

The user will see a section with the main title **Turn on/off a
switch**. A dropdown will be populated below that with all the switches
that have the switch capability (**capability.switch**) for them to
select the one they'd like to use. Their choice is then stored in a
variable named **switch1**.

They will also see a section with the main title of **At what time?**.
An input field will allow them to select their time, because the input
field type is set as a **cron**. Additionally, this input field will
have a title set to **When?**. The date they select is stored in a
variable named **cron1**.

::

    def installed() {
        log.debug "Installed with settings: ${settings}"
        schedule(cron1, "scheduleCheck")
    }

The installed method is trigged upon initial installation. In this
particular case, a message is logged to the console, and then an event
is schedule. The schedule method takes in a time, which in this case is
**cron1**, defined by the user above. It also takes a method name to be
run, which in this case is **scheduleCheck**. Basically it sets up a
cron job to run the method defined at a particular time.

::

    def updated() {
        log.debug "Updated with settings: ${settings}"
        unschedule()
        schedule(cron1, "scheduleCheck")
    }

The updated method is run every time the preferences are changed by the
end user. A messaged is logged to indicate this. **Unschedule** is
called to remove any previously schedule cron jobs, and then a new cron
job is scheduled, just as it is above.

::

    def scheduleCheck()
    {
        log.debug "scheduledCheck: $settings"
        def latestValue = switch1.currentSwitch

        if(latestValue == "off") {
            log.debug "Switch is currently off, switching on"
            switch1.on()
        } else if(latestValue == "on") {
            log.debug "Switch is currently on, switching off"
            switch1.off()
        } else {
            log.debug "Latest value is not on or off, by default turn switch off. Value: $latestValue"
            switch1.off()
        }
    }

This method is called when scheduled via the cron job. First, we get the
current state of the light switch (whether it's on or off). The
possibilites for this value are defined in the capabilities definitions
under the attribute column.
.. (LINK).

We use the name of the variable defined at the top, **switch1**, and
TODO determine how .currentSwitch works. Then we use **latestValue**,
which either has a value of off or on, to determine what to do next. If
the value is off, we log a message, and call the command **on()**. The
inverse happens if the light is currently on.

The documentation on capabilities shows what commands can be called on
devices with particular capabilities. Obviously, in this case a switch
can be turned off or on. There is also a fallback case if the
latestValue isn't either on or off. TODO is that even possible?

`Back to Examples <index.md>`__
