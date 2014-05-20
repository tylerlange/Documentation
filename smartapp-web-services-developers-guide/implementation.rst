SmartThings Web Services Implementation
=======================================

Developing an “API Access” Application
--------------------------------------

**Step 1:** Create a new SmartApp in the IDE. Click on **Enable OAuth in
Smart App** to receive an auto-generated Client ID and Client Secret.
For this example we’ll assume myclient and mysecret. You should also set
an **OAuth Client Display Name**, as it will show up in your
authorization prompt later on.

**Step 2:** All SmartApps must declare preferences metadata that is used
at installation/configuration time to allow the user to control what
devices the SmartApp will actually have access to. This is a
configuration step, but also a security step, whereby the security
context of the SmartApp is established by the end users themselves.

Web Services SmartApps are no different, and this is part of the power
of the approach. The end user controls exactly what devices the SmartApp
will have access to, and therefore what devices the external systems
that consume those web services will have access to.

::

    /**
    * Sample API Access Application
    *
    * Author: SmartThings
    */
    preferences {
      section("Allow External Service to Control These Things...") {
        input "switches", "capability.switch", title: "Which Switches?", multiple: true, required: false
        input "motions", "capability.motionSensor", title: "Which Motion Sensors?", multiple: true, required: false
        input "locks", "capability.lock", title: "Which Locks?", multiple: true, required: false
      }
    }

Also make sure to add the required installed and updated methods

::

    def installed() {

    }

    def updated() {

    }

**Step 3:** The services metadata declaration allows the SmartApp
developer to expose an HTTP endpoint and map the various supported HTTP
operations to an associated event handler.

The event handler can process the HTTP request and provide a response
including both the `HTTP status
code <https://en.wikipedia.org/wiki/List_of_HTTP_status_codes>`__ as
well as the response body.

Add some request mappings in the source code of this new app. There is
no limit on the number of mappings, but the path level is restricted to
4 levels deep (i.e. /level1/level2/level3/level4). The syntax for doing
so is as follows:

::

    mappings {
      path("/switches") {
        action: [
          GET: "listSwitches",
          PUT: "updateSwitches"
        ]
      }
      path("/switches/:id") {
        action: [
          GET: "showSwitch",
          PUT: "updateSwitch"
        ]
      }
    }

**Step 4:** Define event handler methods that will be called upon
matching path/verb combinations. Below is an example that matches one
such combination:

::

    void updateSwitch() {
        def command = request.JSON?.command
        if (command) {
          def mySwitch = switches.find { it.id == params.id }
          if (!mySwitch) {
            httpError(404, "Switch not found")
          } else {
            mySwitch."$command"()
          }
        }
    }

**Step 5:** Self-publish the app from the IDE by clicking on “Publish”
-> “For Me”.

**Step 6:** Obtain an OAuth authorization code.

The authorization code (once returned) will take the place of a password
in the next step of getting the OAuth access token.

Retrieve an authorization code by first authenticating with SmartThings.
Set the client\_id to the value specified in your SmartApp, and the
redirect\_uri to a location you have setup to handle these OAuth 2
requests. For this example, I’ve used a value that corresponds to a
small Sinatra app defined later in this document.

::

    Request:
    GET https://graph.api.smartthings.com/oauth/authorize?response_type=code&client_id=myclient&scope=app&redirect_uri=https%3A%2F%2Fgraph.api.smartthings.com%2Foauth%2Fcallback

The user clicks “Authorize” when prompted.

.. figure:: ../img/smartapps/web-services/preferences.png
   :alt: Alt Authorize

   Alt Authorize
A request with a parameter called “code” will now be sent to
redirect\_uri, which can be used to exchange for an access token in a
separate request, shown below:

.. figure:: ../img/smartapps/web-services/code.png
   :alt: Alt Code

   Alt Code
**Step 7:** Obtain an access token using the grant.

Now that you have the code (CQjHG8 in the URL above), you can make an
HTTP request from the SmartThings OAuth endpoint using: + The Code + The
OAuth Client ID + The OAuth Client Secret

This request will return a JSON document that contains the OAuth2 access
token that you will use to make subsequent requests.

::

    Request:
    GET
    https://graph.api.smartthings.com/oauth/token?grant_type=authorization_code&client_id=myclient&client_secret=mysecret&redirect_uri=https%3A%2F%2Fgraph.api.smartthings.com%2Foauth%2Fcallback&scope=app&code=XXXXX

    Response:
    200 OK
    {
      "access_token": "43373fd2871641379ce8b35a9165e803",
      "expires_in": 1576799999,
      "token_type": "bearer"
    }

Note that while the normal flow of the mechanics of getting the OAuth2
access token are invisible to the enduser, the entire flow can be done
as shown above using nothing but a web browser. This allows you to get
an access token for testing purposes.

**Step 8:** Discover the endpoint URL in the following way, passing the
token to the specified URL.

::

    Headers:
    Authorization: Bearer 43373fd2-8716-4137-9ce8-b35a9165e803

    Request:
    GET /api/smartapps/endpoints

    Response:
    200 OK
    [
      {
      "oauthClient": {
        "clientId": "myclient",
        "authorizedGrantTypes": "authorization_code"
      },
      "url": "/api/smartapps/installations/8a2aa0cd3df1a718013df1ca2e3f000c"
      }
    ]

**Step 9:** Make API calls to your app as follows:

::

    Headers:
    Content-Type:  application/json
    Authentication:  Bearer 43373fd2-8716-4137-9ce8-b35a9165e803

    Request:
    PUT /api/smartapps/installations/8a2aa0cd3df1a718013df1ca2e3f000c/switches/8a818a9b39c0de7f0139c0dff1290073

    Request body:
    {command: on}

    Response:
    204 No Content

**Testing the Application in Your Own Account**

Once you have written your “API Access” SmartApp, you can publish it
into your own personal SmartThings account (into the MyApps Category) to
test it.

You can do this by accessing the “Publish” menu from within the SmartApp
editor as shown below.

.. figure:: ../img/smartapps/web-services/publish.png
   :alt: Alt Code

   Alt Code
**Submitting the Application for Approval**

Once your SmartApp is fully tested with your external system/service,
you can then submit it for approval and publication into the
“Connections” category.

NOTE: This option is currently not available. Send an email to
build@smartthings.com for now when you are ready to publish.

Example OAuth 2 integration app
-------------------------------

This is a small Sinatra app that should allow you to fully connect to a
SmartApp through OAuth 2.

File: server.rb

::

    require 'bundler/setup'
    require 'sinatra'
    require 'oauth2'
    require 'json'

    client_id = 'myclient'
    api_key = 'mysecret'

    redirect_uri = 'http://localhost:4567/oauth/callback'

    options = {
      site: 'https://graph.api.smartthings.com',
      authorize_url: '/oauth/authorize',
      token_url: '/oauth/token'
    }

    client = OAuth2::Client.new(client_id, api_key, options)

    get '/' do
      %(<a href="/authorize">Connect with SmartThings</a>)
    end

    get '/authorize' do
      url = client.auth_code.authorize_url(redirect_uri: redirect_uri, scope: 'app')
      redirect url
    end

    get '/oauth/callback' do
      response = client.auth_code.get_token(params[:code], redirect_uri: redirect_uri, scope: 'app')
      response.token
    end

File: Gemfile source "http://rubygems.org"

::

    gem 'sinatra'
    gem 'oauth2'
    gem 'json'
