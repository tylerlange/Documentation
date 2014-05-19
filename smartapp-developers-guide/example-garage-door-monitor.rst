Example: Garage Door Monitor
============================

::

    /**
     *  Garage Door Monitor
     *
     *  Author: SmartThings
     */
    preferences {
        section("When the garage door is open...") {
            input "multisensor", "capability.threeAxis", title: "Which?"
        }
        section("For too long...") {
            input "maxOpenTime", "number", title: "Minutes?"
        }
        section("Text me at (optional, sends a push notification if not specified)...") {
            input "phone", "phone", title: "Phone number?", required: false
        }
    }

    def installed()
    {
        subscribe(multisensor, "acceleration", accelerationHandler, [filterEvents: false])
    }

    def updated()
    {
        unsubscribe()
        subscribe(multisensor, "acceleration", accelerationHandler, [filterEvents: false])
    }

    /*
    The "acceleration" message comes during acceleration, but also is reported every 2.5 minutes, so we listen for
    that and then check if the garage door has been open for longer than the threshold.
    */
    def accelerationHandler(evt) {
        def latestThreeAxisState = multisensor.threeAxisState // e.g.: 0,0,-1000

        if (latestThreeAxisState) {
            def latestThreeAxisDate = latestThreeAxisState.dateCreated.toSystemDate()
            def isOpen = Math.abs(latestThreeAxisState.xyzValue.z) > 250 

            if (isOpen) {
                def deltaMillis = 1000 * 60 * maxOpenTime
                def timeAgo = new Date(now() - deltaMillis)
                def openTooLong = latestThreeAxisDate < timeAgo
                log.debug "openTooLong: $openTooLong"
                def recentTexts = state.smsHistory.find { it.sentDate.toSystemDate() > timeAgo }
                log.debug "recentTexts: $recentTexts"

                if (openTooLong && !recentTexts) {
                    def openMinutes = maxOpenTime * (state.smsHistory?.size() ?: 1)
                    sendTextMessage(openMinutes)
                }
            }
            else {
                clearSmsHistory()
            }
        }
        else {
            log.warn "COULD NOT FIND LATEST 3-AXIS STATE FOR: ${multisensor}"
        }
    }

    def sendTextMessage(openMinutes) {
        log.debug "$multisensor was open too long, texting $phone"

        updateSmsHistory()
        def msg = "Your ${multisensor.label ?: multisensor.name} has been open for more than ${openMinutes} minutes!"
        if (phone) {
            sendSms(phone, msg)
        }
        else {
            sendPush msg
        }
    }

    def updateSmsHistory() {
        if (!state.smsHistory) state.smsHistory = []

        if(state.smsHistory.size() > 9) {
            log.debug "SmsHistory is too big, reducing size"
            state.smsHistory = state.smsHistory[-9..-1]
        }
        state.smsHistory << [sentDate: new Date().toSystemFormat()]
    }

    def clearSmsHistory() {
        state.smsHistory = null
    }

Let's break it down.

::

    preferences {
      section("When the garage door is open...") {
        input "multisensor", "capability.threeAxis", title: "Which?"
      }

The user will see a section with the main title **When the garage door
is open**. A dropdown will be populated below that with all the devices
that have the threeAxis capability (**capability.threeAxis**) for them
to select the sensor(s) they'd like to use. There is also a title above
the field labeled **Which?**. Their choice is then stored in a variable
named **multisensor**.

::

      section("For too long...") {
        input "maxOpenTime", "number", title: "Minutes?"
      }

The user will see a section with the main title **For too long**. The
input field type of **number** is used, to allow the user to put in a
numeric minute value. Their input is then stored in a variable named
**maxOpenTime**.

::

      section("Text me at (optional, sends a push notification if not specified)...") {
        input "phone", "phone", title: "Phone number?", required: false
      }
    }

The user will see a section with the main title **Text me at (optional,
sends a push notification if not specified)...**. It uses the field type
of **phone** to provide a formatted input for phone numbers. This
particular field is marked as not required, by **required: false**.
There is also a title above the field labeled **Phone Number?**. Their
input is then stored in a variable named **phone**.

::

    def installed()
    {
      subscribe(multisensor, "acceleration", accelerationHandler, [filterEvents: false])
    }

Upon installation, we want to keep track of the status of our garage
door. We use the subscribe method to "listen" to the **acceleration**
attribute of the garage door, as defined as **multisensor**. When the
acceleration status changes of the garage door, the method
**accelerationHandler** will be called. The "acceleration" message comes
during acceleration, but also is reported every 2.5 minutes, so as a
result **accelerationHandler** will be called every 2.5 minutes.
**filterEvents** does TODO

::

    def updated()
    {
        unsubscribe()
        subscribe(multisensor, "acceleration", accelerationHandler, [filterEvents: false])
    }

If anything changes in the user configuration, unsubscribe everything
and resubscribe.

::

    /*
    The "acceleration" message comes during acceleration, but also is reported every 2.5 minutes, so we listen for
    that and then check if the garage door has been open for longer than the threshold.
    */
    def accelerationHandler(evt) {
        def latestThreeAxisState = multisensor.threeAxisState // e.g.: 0,0,-1000

Get the latest state of the sensor, by getting **threeAxisState** which
follows the syntax of attributeName followed by State. TODO Double check
this

::

        if (latestThreeAxisState) {

If we can get the state

::

            def latestThreeAxisDate = latestThreeAxisState.dateCreated.toSystemDate()

See when this state was reported in (TODO add method link)

::

            def isOpen = Math.abs(latestThreeAxisState.xyzValue.z) > 250 // TODO: Test that this value works in most cases...

If the value on the Z plane (absolutely) is greater than 250, than the
garage door is up.

TODO graph of X,Y,Z plane and explanation of what 250 means in the real
world.

::

            if (isOpen) {
                def deltaMillis = 1000 * 60 * maxOpenTime

**deltaMillis** is the number of milliseconds the garage can be open
for.

::

                def timeAgo = new Date(now() - deltaMillis)

**timeAgo** is the time in milliseconds the garage could have been open
and still be okay. Anytime before this, and it would have been open too
long.

::

                def openTooLong = latestThreeAxisDate < timeAgo

If the last time the sensor checked in **(latestThreeAxisDate)** was
before the earliest time allowed by the garage to be open (**timeAgo**)
then the garage has been open too long **(openTooLong)**.

::

                log.debug "openTooLong: $openTooLong"
                def recentTexts = state.smsHistory.find { it.sentDate.toSystemDate() > timeAgo }

See if any texts have been sent since **(timeAgo)**, meaning that the
user has already been notified the garage is up. After the time
specified for the user to be notified, it will send another message. For
example, if you wanted to know if it's up for 10 minutes, it will tell
you every 10 minutes. (TODO CHECK THIS) This uses state and a collection
we created called smsHistory that allows us to query by a particular
value in the collection.

::

                log.debug "recentTexts: $recentTexts"

                if (openTooLong && !recentTexts) {

If the garage has been open too long and the user hasn't been notified.

::

                    def openMinutes = maxOpenTime * (state.smsHistory?.size() ?: 1)

Calculate the number of minutes the door has been open, by taking the
maximum possible number of minutes, times the number of messages that
have been sent.

::

                    sendTextMessage(openMinutes)

Send a text using the helper method

::

                }
            }
            else {
                clearSmsHistory()

Clear text history of the garage is closed

::

            }
        }
        else {
            log.warn "COULD NOT FIND LATEST 3-AXIS STATE FOR: ${multisensor}"
        }
    }

Error with the sensor

::

    def sendTextMessage(openMinutes) {
      log.debug "$multisensor was open too long, texting $phone"

      updateSmsHistory()

Call helper method

::

      def msg = "Your ${multisensor.label ?: multisensor.name} has been open for more than ${openMinutes} minutes!"
      if (phone) {
        sendSms(phone, msg)
      }
      else {
        sendPush msg
      }
    }

Depending on how the user specified, send an SMS or a push notification

::

    def updateSmsHistory() {
      if (!state.smsHistory) state.smsHistory = []

if smsHistory isn't defined yet, set it

::

      if(state.smsHistory.size() > 9) {

Check the current size of state

::

        log.debug "SmsHistory is too big, reducing size"
        state.smsHistory = state.smsHistory[-9..-1]

Sets the smsHistory to the last nine sms entries (a negative index goes
from the back to the front of the collection)

::

      }
      state.smsHistory << [sentDate: new Date().toSystemFormat()]

Add a new value to the sms history, with the current time

::

    }

    def clearSmsHistory() {
      state.smsHistory = null
    }

Clear history
