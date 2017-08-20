# Expose an existing http based api

The fugazi client executes remote commands by issuing http requests which makes it easy to use any service which 
exposes an http api.  
The client comes with an existing set of [commands for making http requests](builtins/commands?id=net-commands) which 
can be used to interact with an existing service, for example:

```fugazi
get "http://myservice.com/api/user/userid"
```

While this approach works (if the service supports [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)), 
it can be tedious and not very intuitive.  
The fugazi terminal was created to as a cli tool for http based services, in order to use it you need to tell fugazi how 
your service looks like.

There are two options for exposing your service to fugazi:
 - [Configure your service](exposeService?id=configuring-your-service)
 - [Using the proxify node package](exposeService?id=using-proxify)

In both cases you'll need to create a [descriptor](descriptors)(s) to your service.  
You don't have to map your entire service, you can start with one or two [commands](components?id=commands) and [built-in types](builtins/types), 
and improve your descriptor by adding more commands, types and other [components](components) as you see fit. 

If you're looking to expose a service/application which isn't exposed to http, then you'll need to use a [connector](connector ":target=_self").

## Configuring your service
Any http exposed service/application can be accessed using fugazi, as long as the endpoints which are requested support CORS.  
By creating a [descriptor](descriptors) you inform the terminal how to interact with your service. 
This json descriptor also needs to be served by the service (because it is also restricted by the browser's cross origin policy).  
If you can't (or don't want to) serve the descriptor from your service you can load it as a javascript file instead of a json file, 
to do so just wrap your json object in the following way:
```javascript
(function() {
	fugazi.components.modules.descriptor.loaded(DECRIPTOR_JSON_OBJECT);
})();
```
The file needs to end with a `.js` extension, and then it can be loaded from anywhere without the CORS restrictions (can be 
hosted locally).

If your service does not support CORS you can still use it if you host the [proxy frame](https://github.com/fugazi-io/webclient/blob/master/html/proxyframe.html) 
which will be used to proxy the requests to your service and bypass the cross origin restrictions.

## Using proxify
[Proxify](https://github.com/fugazi-io/proxify) is a node app that takes care of:
* Handling CORS headers for all requests
* Serving the descriptor file(s)
* Passing the requests to the existing service and returning the response back to the fugazi client

Please check out the [project README](https://github.com/fugazi-io/proxify) for more information.