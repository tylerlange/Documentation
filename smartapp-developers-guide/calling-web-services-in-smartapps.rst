Calling Web Services in SmartApps
=================================

Within SmartApps you will often have to make calls to external web
services, so we have provided a variety of mechanisms for doing that.

When calling an external web service it's important to stay within the
bounds of execution time for a SmartApp. You are limited to 20 seconds.
If your web service takes longer than this to run, then your request
will timeout.

Here are the helper methods available to you with a few examples below.

**httpDelete**, **httpGet**, **httpHead**, **httpPost**, **httpPostJson**, **httpPut**,
**httpPutJson** 

httpGet Example
---------------

::

    def pollParams = [
        uri: "https://api.ecobee.com",
        path: "/1/thermostat",
        headers: ["Content-Type": "text/json", "Authorization": "Bearer ${atomicState.authToken}"],
        query: [format: 'json', body: jsonRequestBody]

Prepare parameters.

::

    try{
        httpGet(pollParams) { resp ->

Send GET request and direct according to response.

::

        if (resp.data) {
            debugEvent ("Response from Ecobee GET = ${resp.data}", true)
            debugEvent ("Response Status = ${resp.status}", true)
            }
            if(resp.status == 200) {
            log.debug "poll results returned"
        }
            else {
            log.error "polling children & got http status ${resp.status}"
        }
    } catch(Exception e)
    {
      log.debug "___exception polling children: " + e
        debugEvent ("${e}", true)
    }

httpPost Example
----------------

::

    def params = [
            uri: 'https://my.example.com',
            body: [param1: myParam1, param2: myParam2]
        ]

Prepare parameters.

::

        httpPost(params) {response ->

Send POST request and set internal state according to third party
device's state.

::

            state.auth = response.data
            state.auth.expires_in = Date.parse('EEE, dd-MMM-yyyy HH:mm:ss z', response.data.expires_in).getTime()

            if(method != null) {
                api(method, args, success)
            }
        }

httpPostJson Example
--------------------

::

    httpPostJson(uri: deviceInfo.callbackUrl, path: '',  body: [evt: [deviceId: evt.deviceId, name: evt.name, value: evt.value]]) {
            log.debug "Event data successfully posted"
        }
