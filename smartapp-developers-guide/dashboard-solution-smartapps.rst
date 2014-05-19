Dashboard Solution SmartApps
============================

Solution Module Meta-Data
-------------------------

The first step in setting up a Solution Module SmartApp is to setup your
preferences. As with other types of SmartApps, preferences define the
user inputs shown when your SmartApp is installed for the first time or
when the enduser is reconfiguring. They are defined within a preferences
block.

::

    preferences {}

Find more information on the basic structure of preferences `go
here <../smartapp-developers-guide/preferences-&-settings.md>`__.

Dashboard solution apps utilize additional capabilities within
meta-data.

| **Cards**
| Cards influence the end user experience in a similar way to pages.
They allow you to swipe through various panes.

| **Tiles**
| There are four types of tiles that can be included in cards 

.. (TODO CHECK THIS).

-  stateTile: Shows current state of events 
.. TODO add link
-  eventTile: Shows past events 
.. TODO add link
-  exploreTile: Shows static photo with an optional link 
.. TODO add link.
   Note that when creating an exploreTile it references our personal
   CDN. You will need to store your files externally to be able to
   utilize them.
-  smartAppGroupTile: Allows you to change the states of a thing on and
   off 
.. TODO add link

   cards { card(name:"Right Now", sortable:false) { tiles { stateTile {
   } } } card("Recently") { tiles { eventTile { } } } }

Parent/Child SmartApps
----------------------

Dashboard solution SmartApps introduce hierarchy into their structure.
Using parent and children relationships allows for more organized,
paginated user interfaces. Within a top level SmartApp you can define
child apps, like in this example.

::

    dynamicPage(name: "areaDetail", title: "What do you want to do when there is activity in the $app.label?", install: true, popToAncestor: "all") {
            section {
                app "motionAlerts", "SmartSolutions/Motion", "Motion Alerts", title:"Get notified when there is activity", page: "motionAlerts", multiple: false, install: true
            }
            if (camera) {
                section {
                    app "motionPhotoActivity", "SmartSolutions/Motion", "Motion Photo Activity", title:"Take photos when there is activity", page: "motionPhotoActivity", multiple: false, install: true
                }
                section {
                    app "motionPhotoPeriodic", "SmartSolutions/Motion", "Motion Photo Periodic", title:"Take photos periodically", page: "motionPhotoPeriodic", multiple: false, install: true
                }
            }
        }

This code initializes one app by default, and conditionally initializes
another two if there is a camera selected. 
.. TODO Link Method

When initializing the app you will need to define the namespace. Use
your github username followed by the name of your SmartApp, like this:
username/AppName.

For cases where some of the required data is either not know when the
app is loaded, or for cases where multiple children need to be created
in a single instant, the method addChild can be used. LINK IT OUT

Calling Methods Between SmartApps
---------------------------------

Calling a method on a parent SmartApp from a child SmartApp is quite
simple. You simply call the method on parent like so:

::

    parent.methodName();

Calling a method on a child SmartApp from a parent SmartApp requires you
to first find the child(ren) you'd like to alter using the childApps
variable. After finding them, then you can call a method on them.

::

    childApps.each {child ->
        if(child.attributeName=="sampleAttributeName") {
            child.methodName();
        }
    }

Sharing Information Between SmartApps
-------------------------------------

You can access pass information between parent and child SmartApps in a
similar way to how you call methods above.

Getting parent's attribute within a child.

::

    parent.attributeName;

Getting a child's attribute within a parent.

::

    childApps.each {child ->
        attributeValue = child.attributeName;
    }

Sending Events From Your SmartApp
---------------------------------

The sendEvent method allows you to send events from your dashboard
solution SmartApps.

::

    sendEvent(linkText:app.label, descriptionText:descriptionText, eventType:"SOLUTION_EVENT", displayed: false, name:"summary", value:value, data: getSolutionEventData(value))

.. TODO this should be added to the method library itself.

sendEvent can take the following parameters:

-  **linkText:** A substring of the descriptionText to highlight/bold in
   the event feed.
-  **descriptionText:** A text description for the event that will show
   up in the event feed.
-  **eventType:** We support a number of event types (SOLUTION\_SUMMARY,
   SOLUTION\_EVENT, SOLUTION\_STATE, NOTIFICATION,
   LOCATION\_MODE\_CHANGE, IMAGE, ALERT), each of which have a purpose
   in our mobile apps:

   -  SOLUTION\_SUMMARY: Events that should contribute to the solution
      summary card in the Dashboard (summary on the top of each module)
   -  SOLUTION\_EVENT: Events that should contribute to the historical
      view of solution events in the Dashboard ("Recently" card)
   -  SOLUTION\_STATE: Events that should contribute to the current
      state of configured items in each solution ("Right Now" card)
   -  NOTIFICATION: Events that should be shown as notifications in
      Hello, Home
   -  LOCATION\_MODE\_CHANGE: These are generated by the platform when a
      Location mode changes.
   -  IMAGE: After taking a photo with a camera device, events of this
      type tell mobile apps to directly render image data from image
      URL's included in the event.data field.
   -  ALERT: Events to show important alerts to users. We currently use
      it for Z-Wave lock secure inclusion failures, with a message of,
      "This lock failed to complete the network security key exchange.
      If you are unable to control it via SmartThings, you must remove
      it from your network and add it again."
      displayed: This boolean value controls whether or not the event
      should be displayed in the user's event feed.

-  **value:** This is the value of the event, e.g. "on", "off", 74,
   "active", etc. It is also highlighted/bolded in descriptionText in
   the event feed.
-  **data:** This is unstructured JSON data that can be used for
   multiple purposes, you can find examples in the SmartApps repo.