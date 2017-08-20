# Connector

The fugazi client can only execute commands over http(s), which makes it simple for services/applications 
which expose http based API.  
However, applications and services which do not expose an http interface cannot be accessed by fugazi, which is where
the connectors come in.  
A connector needs to expose http endpoints for the [descriptors](../#descriptors ":target=_self") and 
[remote commands](../#components?id=commands ":target=_self") and map between each command endpoint to the appropriate command execution.  

To make things simple we've created a node package which takes care of everything needed for such a connector.  

## Do I need a connector?
If the service which you want to use from the fugazi client is already exposed to http then you do not need to use a 
connector, instead it's enough to serve a descriptor which describes your existing exposed api.  
More info in [Expose an existing http based api](../#exposeService ":target=_self").

## Where to find
 - GitHub repo: [https://github.com/fugazi-io/connector.node](https://github.com/fugazi-io/connector.node)
 - npm package: [https://www.npmjs.com/package/@fugazi/connector](https://www.npmjs.com/package/@fugazi/connector)

## What's included
 - Http server to serve descriptors and remote commands
 - Easy way of adding components (modules, types, commands, etc)
 - Automatically creates the descriptors
 - Supports both CORS and proxy frame
 - A ready to use logger

## Installing & using
Please consult the [project README](ttps://github.com/fugazi-io/connector.node).

