Preferences & Settings
======================

Static Preferences
------------------

As a SmartApps developer, you will define preferences that tell us what
kind of Things and other information you need in order for the
application to be able to run. We’ll then use that preferences metadata
to collect that information from the end user at the time of
installation for the SmartApp. The metadata is structured in the
following hierarchy:

-  pages: A list of sections, containing meta data about the page flow.

   -  sections: A list of maps each containing 'title' and 'input' keys

      -  title: A string that will be displayed to the user describing
         the section. Ex. "Choose your lamp", "Remind me at..."
      -  input: Contains a map that holds the section's variables

         -  name: A string that represents the variable name you will
            use in your code. Ex. "phone1", "switch2"
         -  title: A string that will be displayed to the user
            describing the selection box. Ex. "Where?", "What time?",
            "Phone number?"
         -  type: A string describing the type of variable. One of
            device.openClosedSensor, device.doorShield,
            device.lightSensor, device.motionDetector,
            device.onoffShield, device.particulateDetector,
            device.petFeederShield, device.smartSenseMulti,
            device.smartSenseMotion, device.smartPhone,
            device.smarttagMotionSensor, device.smarttagPresenceSensor,
            device.smarttagTemperatureSensor,
            device.smarttagThreeAxisSensor, device.smartthingShield,
            device.spark, device.switch, device.temperatureSensor,
            device.unknown, device.aeonMultiSensor, device.zWaveAlarm,
            device.zWaveMultilevelSwitch, device.zWaveSwitch,
            device.zWaveWaterAlarm, time, phone, number, string
         -  description: A string that will be displayed to the user
            that will be the default selection. Ex. "Tap to set"
         -  multiple: A boolean that sets whether the selection box
            allows
         -  required: A boolean that sets if this input must be selected
            by the user. Ex. true, false multiple devices to be
            selected. Ex. true, false

      -  app: A child app to include.
      -  label: A string to display.
      -  mode: An option field for user to select mode.
      -  paragraph: A string to display for messaging purposes.
      -  icon: An icon to display.
      -  href: A link.
      -  buttons: Button to display.
      -  image: An image to display.

.. TODO Link to methods

The final output takes a format like this

::

    preferences {
        section("When the door opens/closes...") {
            input "contact1", "capability.contactSensor"
        } section("Turn on these lights...") {
            input "switch1", "capability.switch", title: "Which door?", multiple: true
        }
    }

This preferences definition will ask the user for two bits of
information: a device with a contact sensor capability to use and a
device with an on/off switch capability to use. Note that we are asking
for devices by capability and NOT by type. This is important since
device types are always protocol specific and may even be manufacturer
or model specific. If we want to write the most flexible SmartApp, we
need to forget about device types and embrace device capabilities.

The preferences metadata defined in the application itself provides all
of the information we need to render and collect these preferences from
the enduser at the time they install (and configure) the application.

As a developer, you don’t know or care what the enduser called their
devices. You get to name them within the scope of your application so
that you can use those names whenever you reference the device.

Of course users can change their preferences later as well, and
applications need to know how to react to those changes. Notice also
that the preferences gave internal (appspecific) names to the Things
that are used by the application (e.g. contact1 for the contact sensor
and switch1 for the onoff switch).

If you'd like a paginated experience for the end user, pages influence
the end user experience. They allow you to organize the initial install
screens into separate pages. Each app can create one level of pages, and
their children can do the same.

There are two different types of pages that can be created. You can
simply create pages, like so

::

    preferences {
        page( name:"page1", title:"Preferences Page 1", nextPage:"page2", uninstall:true, install:false ) {
            section( "When a door opens..." ) {
                input "door1", "capability.contactSensor", title:"Where?"
            }
        }

        page( name:"page2", title:"Preferences Page 2", uninstall:true, install:true ) {
            section( "Turn on a light..." ) {
                input "switch1", "capability.switch", title:"Which light?"
            }
        }
    }

You could also create dynamic pages, which involve variable content.

.. TODO Link Method

Dynamic Preferences
-------------------

Dynamic preferences are used in the context of dynamic pages. These
could change based on previous inputs and is common in the
dashboard/solution smart apps. For example, if you allow the user to
choose between different types of devices, you could display different
preferences for the chosen type.

::

    preferences {
        page(name: "lightDetail")
    }

    def lightDetail() {
        dynamicPage(name: "lightDetail", title: "Configure $app.label", nextPage: "lightOptions") {
            section {
                input "switches", "capability.switch", title: "Choose devices for $app.label", multiple: true, required: true, pairedDeviceName: nextPairedDeviceName("$app.label", switches)
                icon title: "Choose an icon for $app.label", required: true, defaultValue: "st.Lighting.light13-icn"
            }
        }
    }

Preferences Data Types
----------------------

Devices

Specific devices like a zWave Alarm or SmartMotion detector. Use
'device' prefix when declaring. Ex. "device.motionDetector" or
"device.switch"

Supported Device Types:

contactSensor, lightSensor, motionDetector, smartContact, smartMotion,
smartPhone, smarttagPresenceSensor, switch, temperatureSensor,
zwaveAeonMultisensor, doorShield, onOffShield, particulateDetector,
petFeederShield, zwaveAlarm, zwaveWaterAlarm, zwaveMultilevelSwitch,
zwaveSwitch, smartthingShield, zwaveThermostat

Capabilities

Instead of a specific device, an ability that multiple devices could
have. For example both the smarttagPresenceSensor and smartPhone devices
have the presenceSensor capability. Use 'capability' prefix when
declaring. Ex. "capability.presenceSensor" or "capability.switch"

Supported Capabilities:

switch, battery, contactSensor, motionSensor, illuminanceMeasurement,
temperatureMeasurement, relativeHumidityMeasurement, presenceSensor,
alarm, waterSensor, threeAxisMeasurement, polling, configuration,
thermostatHeatingSetpoint, thermostatCoolingSetpoint,
thermostatSetpoint, thermostatMode, thermostatFanMode

Primitive Input Types

Other types of user inputted data. Depending on the type, the user will
be shown a different interface upon app installation. A keyboard for
'text', a number pad for 'number' or 'phone', and a time picker for
'time'

Supported Types:

text, number, decimal, time, phone

.. TODO: Make this auto pull (Philip Grey)

