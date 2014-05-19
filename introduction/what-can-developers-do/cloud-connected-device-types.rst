Cloud Connected Device Types
----------------------------

Cloud-connected devices are devices that are managed by a third-party
cloud service, whether it's operated by the device manufacturer (e.g,
Quirky Wink, Ecobee thermostat) or a third-party aggregator (e.g.,
Xively, ThingWorx).

SmartThings support of cloud-connected devices is limited to those whose
cloud services authenticate through a standard OAuth2 flow, and
communicate via HTTP-based APIs. While it is technically possible to
store and encypt of the user's credentials for the remote service rather
than completing an OAuth2 flow and storing only an access token, it is
highly discouraged.
