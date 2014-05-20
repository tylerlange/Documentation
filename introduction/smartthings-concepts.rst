Smart Things Important Concepts
===============================

Within the SmartThings platform, there are three different “containers”
that are important concepts to understand. These are: accounts,
locations, and groups. These containers represent both security
boundaries and navigation containers that make it easy for users to
browse their devices.

The diagram below shows the hierarchical relationship between these
containers. Each type of container is described below in more detail.

.. figure:: ../img/overview/container-hierarchy.png
   :alt: Container Hierarchy

   Container Hierarchy

Asynchronous & Eventually Consistent Programming
------------------------------------------------

When dealing with the physical graph there will always be a delay between when you request something to happen and when it actually happens. There is latency in all networks, but it's especially pronounced when dealing with the physical graph.

To deal with this, the SmartApps platform utilizes asynchronous execution. This means that anytime you execute a command, it doesn't stop everything else from running. This helps everyone's code run the most efficiently.

Our basic methodology towards executing a command, such as turning a light switch on, is "fire and forget". This means that you execute a command, and assume it will turn on in due time, without any sort of follow up.

You cannot be guaranteed that your command has been executed, because another SmartApp could interact with your end device, and change it's state. For example, you might turn a light switch on, but another app might sneak in and turn it off.

If you needed to know if a command was executed, you can subscribe to an event trigged by the command you executed and check it's timestamp to ensure it fired after you told it to. You will, however, still have latency issues to take into consideration, so it's impossible to know the exact current status at any given time.

TODO example

The SmartApps platform follows eventually consistent programming, meaning that responses to a request for a value in SmartApps will eventually be the same, but in the short term they might differ.

**Future State**

In the future, we'd like to move towards providing levels of consistency for the end user, so you could specify how consistent you need your data to be.

Also, as we move some of our logic into the hub, we may consider allowing blocking methods (synchronous) as they wouldn't weigh down our network as a whole.

Accounts
--------

Accounts are the top-level container that represents the SmartThings
‘customer’. Accounts contain only Locations and no other types of
objects.

Locations & Users
-----------------

Locations are meant to represent a geo-location such as “Home” or
“Office”. Locations can optionally be tagged with a geo-location
(lat/long). In addition, Locations don’t have to have a SmartThings Hub,
but generally do. Finally, locations contain Groups or Devices.

Groups
------

Groups are meant to represent a room or other physical space within a
location. This allows for devices to be organized into groups making
navigation and security easier. A group can contain multiple devices,
but devices can only be in a single group. Further, nesting of groups is
not currently supported.

Hubs
----

The SmartThings Hub connects directly to your broadband router and
provides communication between all connected Things and the SmartThings
cloud and mobile application.

Devices
-------

Devices are the bones of the physical graph; they are motion sensors,
locks, thermostats, and everything else in between. They are the
connectors between the physical and digital world.

Device-Types
------------

A device type can be as broad as the category it falls under (such as
switches) or as specific as the device itself (such as a WeMo switch).

Device-Type Handlers
--------------------

Device Type Handlers are the glue; the code between the device type and
our SmartThings infrastructure. It allows us to abstract devices by
their capabilities, and handles all the device type's specific
attributes and code needs so the SmartApp developer doesn't have to.

SmartApps
---------

SmartApps are applications created by SmartThings and the SmartThings
community as a whole. They allow a user to tap into the capabilities of
their devices to automate their lives, and can be installed via the
SmartThings mobile applications.

Installed SmartApps
-------------------

An installed SmartApp is an instance of a SmartApp specific to an
individuals configuration and SmartThings account.