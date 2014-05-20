Integrating With SmartThings using APIs
=======================================

Core API
--------

The Core API is what's used to build SmartThings clients. We use it
internally to power our Android and iOS applications, and it allows for
advanced SmartThings integration.

We are in the process of opening up our Core API to developers. We
currently have a partner level scope, where a SmartThings partner can
build an interface using the Core API and can interact with our data at
the request of their users. This API currently requires a partner client
and secret id. `Contact
us <https://smartthings.wufoo.com/forms/partnership-inquiries-x1owr2qt07z2kxo/>`__
if you are interested in becoming a SmartThings partner.

We are also currently developing a user level scope, where a user can
build an interface and have access to their own account only.

Event Streaming API
-------------------

The Event Streaming API allows a client to be able to receive streaming
updates from the SmartThings platform. This API works by connecting
through a socket and receiving JSON responses.

This API currently requires a partner client and secret id.

Custom SmartApp APIs
--------------------

Within SmartApps, you can expose API endpoints that allow you (and other
users) to interact with your SmartApp through OAuth. Through these
endpoints, you can initiate methods within your SmartApp that can
interact with your devices, all from the web.

Calling Outbound Web Services
-----------------------------

There a variety of helper methods you can use within a SmartApp to
integrate with a third party API. For APIs that require authentication,
this involves initiating an OAuth connection and setting up endpoints to
handle authentication responses. From there, you can make any type of
request (POST,GET,PUT, etc) that you need and parse through the
response.