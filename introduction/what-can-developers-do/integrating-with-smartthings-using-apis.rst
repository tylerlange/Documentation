Integrating With SmartThings using APIs
=======================================

Core API
--------

The Core API is what's used to build SmartThings clients. We use it
internally to power our Android and iOs applications, and it allows for
advanced SmartThings integration.

Event Streaming API
-------------------

The Event Streaming API allows a client device to be able to receive
streaming updates from the SmartThings platform. This API works by
connecting through a socket and receiving JSON responses.

Custom SmartApp APIs
--------------------

Within SmartApps, you can expose API endpoints that allow you (and other
users) to interact with your SmartApp through OAuth. Through these
endpoints, you can initiate methods within your SmartApp that can
interact with your devices, from the web.

Calling Outbound Web Services
-----------------------------

There a variety of helper methods you can use within a SmartApp to
integrate with a third party API. For APIs that require authentication,
this involves initiating an OAuth connection and setting up endpoints to
handle authentication responses. Then you can make any type of request
(POST,GET,PUT, etc) that you need and parse through the response.
