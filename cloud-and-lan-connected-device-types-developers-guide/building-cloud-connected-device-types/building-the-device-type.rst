Cloud-Connected Device Types: Building the Device-Type
======================================================

Why No Parse Method?
--------------------

The parse method for cloud connected devices will always be empty. In a
cloud connected device, event data is passed down from the service
manager, not from the device itself, so the parsing is handled in a
separate method. The device type handler doesn't interface directly with
a hardware device, which is what parse is used for.

Invoking Methods on the Parent Service Manager
----------------------------------------------

To invoke a method on the parent service manager, you simply need to
call it in the following format:

::

    parent.methodName()

Sending Commands to the Third-Party Cloud
-----------------------------------------

As with any other device-type, you need to define methods for all of the
possible commands for the capabilities you'd like to support. Then when
a user calls this method, it will pass information up to the parent
service manager, who will make the direct connection to the third party
cloud. You might for example want to turn a switch on, so you would call
the following.

::

    def on() {
      parent.on(this)
    }

Receiving Events from the Third-Party Cloud
-------------------------------------------

The device type handler continuously polls the third-party cloud through
the service manager to check on the status of devices. When an event is
fired, they can then be passed to the child device handler. Note that
poll runs every 10 minutes for Service Manager SmartApps.

In the device-type handler

::

    def poll() {
        results = parent.pollChildren()
        parseEventData(results)
    }

    def parseEventData(Map results){
        results.each { name, value -> 
            //Parse events and optionally create SmartThings events
        }
    }

In the service manager

::

    def pollChildren(){
        def pollParams = [
            uri: "https://api.thirdpartysite.com",
            path: "/device",
            requestContentType: "application/json",
            query: [format:"json",body: jsonRequestBody]

        httpGet(pollParams) { resp -> 
            httpGet(pollParams) { resp -> 
                state.devices = resp.data.devices { collector, stat -> 
                def dni = [ app.id, stat.identifier ].join('.')
                def data = [
                    attribute1: stat.attributeValue,
                    attribute2: stat.attribute2Value
                ]
                collector[dni] = [data:data]
                return collector
                }
            }
    }

Generating Events at the Request of the Service Manager
-------------------------------------------------------

You won't generate events directly within the Service Manager, but
rather request that they are generated within the Device-type handler.
For example:

In the service manager

::

    childName.generateEvent(data)

In the device-type handler

::

    def generateEvent(Map results) {
      results.each { name, value ->
        sendEvent(name: name, value: value)
      }
      return null
    }



Using Device-Type Preferences
-----------------------------

You can add preferences to your device type for extra configuration,
similar to how you'd add preferences to a SmartApp. Learn more about
preferences
`here <http://smartthings.readthedocs.org/en/latest/smartapp-developers-guide/preferences-and-settings.html>`__.

::

    preferences {
        input(type: "enum", name: "variableName", title: "Choose your value", options: variableOptions(), defaultValue: "Option1", style: "segmented")
    }
        


