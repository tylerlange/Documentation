Example: Bon Voyage
===================

::

    /**
     *  Bon Voyage
     *
     *  Author: SmartThings
     *  Date: 2013-03-07
     *
     *  Monitors a set of presence detectors and triggers a mode change when everyone has left.
     */

    preferences {
        section("When all of these people leave home") {
            input "people", "capability.presenceSensor", multiple: true
        }
        section("Change to this mode") {
            input "newMode", "mode", title: "Mode?"
        }
        section("False alarm threshold (defaults to 10 min)") {
            input "falseAlarmThreshold", "decimal", title: "Number of minutes", required: false
        }
        section( "Notifications" ) {
            input "sendPushMessage", "enum", title: "Send a push notification?", metadata:[values:["Yes","No"]], required:false
            input "phone", "phone", title: "Send a Text Message?", required: false
        }
    }

    def installed() {
        log.debug "Installed with settings: ${settings}"
        log.debug "Current mode = ${location.mode}, people = ${people.collect{it.label + ': ' + it.currentPresence}}"
        subscribe(people, "presence", presence)
    }

    def updated() {
        log.debug "Updated with settings: ${settings}"
        log.debug "Current mode = ${location.mode}, people = ${people.collect{it.label + ': ' + it.currentPresence}}"
        unsubscribe()
        subscribe(people, "presence", presence)
    }

    def presence(evt)
    {
        log.debug "evt.name: $evt.value"
        if (evt.value == "not present") {
            if (location.mode != newMode) {
                log.debug "checking if everyone is away"
                if (everyoneIsAway()) {
                    log.debug "starting sequence"
                    runIn(findFalseAlarmThreshold() * 60, "takeAction", [overwrite: false])
                }
            }
            else {
                log.debug "mode is the same, not evaluating"
            }
        }
        else {
            log.debug "present; doing nothing"
        }
    }

    def takeAction()
    {
        if (everyoneIsAway()) {
            def threshold = 1000 * 60 * findFalseAlarmThreshold() - 1000
            def awayLongEnough = people.findAll { person ->
                def presenceState = person.currentState("presence")
                def elapsed = now() - presenceState.rawDateCreated.time
                elapsed >= threshold
            }
            log.debug "Found ${awayLongEnough.size()} out of ${people.size()} person(s) who were away long enough"
            if (awayLongEnough.size() == people.size()) {
                //def message = "${app.label} changed your mode to '${newMode}' because everyone left home"
                def message = "SmartThings changed your mode to '${newMode}' because everyone left home"
                log.info message
                send(message)
                setLocationMode(newMode)
            } else {
                log.debug "not everyone has been away long enough; doing nothing"
            }
        } else {
            log.debug "not everyone is away; doing nothing"
        }
    }

    private everyoneIsAway()
    {
        def result = true
        for (person in people) {
            if (person.currentPresence == "present") {
                result = false
                break
            }
        }
        log.debug "everyoneIsAway: $result"
        return result
    }

    private send(msg) {
        if ( sendPushMessage != "No" ) {
            log.debug( "sending push message" )
            sendPush( msg )
        }

        if ( phone ) {
            log.debug( "sending text message" )
            sendSms( phone, msg )
        }

        log.debug msg
    }

    private findFalseAlarmThreshold() {
        (falseAlarmThreshold != null && falseAlarmThreshold != "") ? falseAlarmThreshold : 10
    }

Let's break it down.

::

    preferences {
      section("When all of these people leave home") {
        input "people", "capability.presenceSensor", multiple: true
      }

The user will see a section with the main title **When all of these
people leave home**. A dropdown will be populated below that with all
the devices that have the presenceSensor capability
(**capability.presenceSensor**) for them to select the sensor(s) they'd
like to use. **Multiple: true** allows them to add as many sensors as
they'd like. Their choice(s) are then stored in a variable named
**people**.

::

      section("Change to this mode") {
        input "newMode", "mode", title: "Mode?"
      }

The user will also see a section with the title **Change to this mode**.
The input field type of **mode** is used, so a dropdown will be populated 
with all the modes the user has set up. There is also a title above the field 
labeled **Mode?**.

::

      section("False alarm threshold (defaults to 10 min)") {
        input "falseAlarmThreshold", "decimal", title: "Number of minutes", required: false
      }

They can now select the number of minutes to wait before the mode change
is enacted. These types of thresholds are common in our SmartApps. A
section is shown titled **False alarm threshold (defaults to 10 min**.
The input field type of **decimal** is used, to allow the user to put in
a numeric minute value. A title of **Number of minutes** is shown above
the field. This particular field is marked as not required, by
**required: false**. By default, all fields are required, so you must
explicitly state if they're note required. Finally, whatever the user
inputs is stored as **falseAlarmThreshold** to be used later.

::

      section( "Notifications" ) {
        input "sendPushMessage", "enum", title: "Send a push notification?", metadata:[values:["Yes","No"]], required:false
        input "phone", "phone", title: "Send a Text Message?", required: false
      }
    }  

Finally, a section is shown labeled as **Notifications**. From here an
input with the field type of **enum** is created. With **enum** you must
define values for it, so they are defined via
**metadata:[values:["Yes","No"]]**. This field is not required as
dictated by **required:false** and what the user selects will be stored
in **sendPushMessage**. There is also an optional field called **Send a
Text Message**. It uses the field type of **phone** to provide a
formatted input for phone numbers. Whatever the user inputs is stored in
the **phone** variable.

::

    def installed() {
        log.debug "Installed with settings: ${settings}"
        log.debug "Current mode = ${location.mode}, people = ${people.collect{it.label + ': ' + it.currentPresence}}"
        subscribe(people, "presence", presence)
    }

Upon installation, we want to keep track of the status of our people. We
use the **subscribe** method to "listen" to the **presence** attribute
of the predefined group of presence sensors, **people**. When the
presence status changes of any of our people, the method **presence**
(the last parameter above) will be called.

::

    def updated() {
        log.debug "Updated with settings: ${settings}"
        log.debug "Current mode = ${location.mode}, people = ${people.collect{it.label + ': ' + it.currentPresence}}"
        unsubscribe()
        subscribe(people, "presence", presence)
    }

If anything changes in the user configuration, unsubscribe everything
and resubscribe.

::

    def takeAction()

This method is called once it's determined that everyone is away, and
after the time set in the **falseAlarmThreshold**.

::

    {
        if (everyoneIsAway()) {

Check again if **everyoneIsAway**; something may have changed in the
time since it was originally called, because of the
**falseAlarmThreshold**.

::

            def threshold = 1000 * 60 * findFalseAlarmThreshold() - 1000

Define the threshold for internal use in this method

::

            def awayLongEnough = people.findAll { person ->
                def presenceState = person.currentState("presence")
                def elapsed = now() - presenceState.rawDateCreated.time
                elapsed >= threshold
            }

Defines a collection by using a groovy method called findAll. findAll
has a closure defined inside it and adds an item to the collection if it
resolves to true. In this case, the **people** collection
is iterated, and a **person** is set. For each person, **presenceState**
grabs the persons current state. The current state includes extra
information about their state, which allows us on the next line to get
the time elapsed since the event was triggered with
**presenceState.rawDateCreated.time** and set it to **elapsed**. We
return true of false, depending on whether the elapsed time is greater
than or equal to the **threshold** defined. If it resolves as true, it's
added to the awayLongEnough collection.

::

            log.debug "Found ${awayLongEnough.size()} out of ${people.size()} person(s) who were away long enough"
            if (awayLongEnough.size() == people.size()) {

If all people have been away long enough

::

                //def message = "${app.label} changed your mode to '${newMode}' because everyone left home"
                def message = "SmartThings changed your mode to '${newMode}' because everyone left home"
                log.info message
                send(message)

Send a message through the channels defined in the preferences

::

                setLocationMode(newMode)

Set the location mode to the mode defined in the preferences

::

            } else {
                log.debug "not everyone has been away long enough; doing nothing"

If any of the people haven't been away long enough

::

            }
        } else {
            log.debug "not everyone is away; doing nothing"
        }
      }

If any of the people aren't away.

::

    def presence(evt)

This method is called when a defined users presence status is changed.

::

      {
        log.debug "evt.name: $evt.value"
        if (evt.value == "not present") {

The presence capability can either be defined as "not present" or
"present" so we check if the value is not present. If the user is
present, than we don't need to check and see if everyone else isn't and
the end event isn't run.

::

            if (location.mode != newMode) {

Checking to see if the desired mode isn't already the same as the
current mode.

::

                log.debug "checking if everyone is away"
                if (everyoneIsAway()) {

Calls method to see if not just this person, but everyone is away

::

                    log.debug "starting sequence"
                    runIn(findFalseAlarmThreshold() * 60, "takeAction", [overwrite: false])

We use the method **runIn**, which runs the method
**takeAction** in a specified amount of time, which in this case is the
return value of the helper method **findFalseAlarmThreshold()**
multiplied by **60** to convert minutes to seconds. **overwrite: false**
makes it so you won't overwrite previously scheduled takeAction calls.
In the context of this SmartApp, it means that if one user leaves, and
then another user leaves within the **falseAlarmThreshold** time,
takeAction will still be called twice. By default, overwrite is true,
meaning that if you scheduled takeAction to run previously, it would be
cancelled and replaced by your current call.

::

                }
            }
            else {
                log.debug "mode is the same, not evaluating"
            }
        }
        else {
            log.debug "present; doing nothing"
        }
    }

If either the mode is staying the same, or the user is present, log
messages.

::

    private everyoneIsAway()
    {
        def result = true
        for (person in people) {
            if (person.currentPresence == "present") {
                result = false
                break
            }
        }
        log.debug "everyoneIsAway: $result"
        return result
    }

Take the **people** variable and iterate through it. Use the method
**currentPresence** on each **person** to get the current attribute
value of their presence capability. If they are present, set the result
to false and break out of the for loop, because you need to search no
more. This would return false as a whole. If you make it all the way
through the loop without any person being present, then you would return
true.

::

    private send(msg) {
        if ( sendPushMessage != "No" ) {
            log.debug( "sending push message" )
            sendPush( msg )
        }

        if ( phone ) {
            log.debug( "sending text message" )
            sendSms( phone, msg )
        }

        log.debug msg
    }

We check the value of **sendPushMessage**, and if it's not **"No"**,
then call the method **sendPush**. All you need to pass along to
sendPush it the message that you'd like to push. We also check to see if
phone was ever defined to a number. If it was, we use the **sendSms**
method to send a message to a particular phone number.

::

    private findFalseAlarmThreshold() {
        (falseAlarmThreshold != null && falseAlarmThreshold != "") ? falseAlarmThreshold : 10
    }

This helper method is used as a getter for the falseAlarmThreshold. If
it's not set, it sets falseAlarmThreshold automatically to **10**
minutes.
