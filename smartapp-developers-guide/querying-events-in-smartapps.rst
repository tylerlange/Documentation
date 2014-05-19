Querying Events in SmartApps
============================

There are a variety of ways to query events within SmartThings, which
can help add historical context to your SmartApps.

.. TODO Link Methods

**List events(Map params=[:])** Returns a list of all events for device,
taking in optional params Supported param keys: ['floor', 'ceiling',
'sinceDate', 'beforeDate', 'max']

::

    def allEvents = device1.events()

**List eventsSince(Date since, Map params=[:])** Returns a list of
events between current time and 'since' date, taking in optional params
Supported param keys: ['floor', 'ceiling', 'sinceDate', 'beforeDate',
'max']

::

    def yesterday = new Date() - 1
    def eventsSinceYesterday = device1.eventsSince(yesterday)

**List eventsBetween(Date floor, Date ceiling, Map params=[:])** Returns
a list of events between 'floor' and 'ceiling' dates, taking in optional
params Supported param keys: ['floor', 'ceiling', 'sinceDate',
'beforeDate', 'max']

::

    def twoDaysAgo = new Date() - 2
    def yesterday = new Date() - 1
    def eventsBetweenPeriod = device1.eventsBetween(twoDaysAgo,yesterday)

Example
=======

This particular example queries events in the past four seconds, and
determines whether a "double tab" happened, to trigger light switches.

::

    def recentStates = master.eventsSince(new Date(now() - 4000), [all:true, max: 10]).findAll{it.name == "switch"}
    if (evt.isPhysical()) {
        if (evt.value == "on" && lastTwoStatesWere("on", recentStates, evt)) {
            onSwitches()*.on()
        } else if (evt.value == "off" && lastTwoStatesWere("off", recentStates, evt)) {
            offSwitches()*.off()
        }
    }

    private lastTwoStatesWere(value, states, evt) {
        def result = false
        if (states) {
                def onOff = states.findAll { it.isPhysical() || !it.type }
                result = onOff.size() > 1 && onOff[0].value == value && onOff[1].value == value
            }
        result
    }