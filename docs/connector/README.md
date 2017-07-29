# Connector

> A node package for exposing server side applications to the fugazi client

In order to use the fugazi client a (remote) service needs to expose an http API.  
To remove the burden of creating we've created this package which implements all that is needed, except for the actual 
logic of executing the service functionality.

## What's included
 - Http server to serve descriptors and remote commands
 - Easy way of adding components (modules, types, commands, etc)
 - Automatically creates the descriptors
 - Supports both CORS and proxy frame
 - A ready to use logger
 
## Where to find
 - GitHub repo: [https://github.com/fugazi-io/connector.node](https://github.com/fugazi-io/connector.node)
 - npm package: [https://www.npmjs.com/package/@fugazi/connector](https://www.npmjs.com/package/@fugazi/connector)

## Installing
```bash
npm install @fugazi/connector
```

## Building & Running
The package comes with a (very) simple example to illustrate what the connector does.  
The example includes two commands which expects a value and return this value, one version is a local command and 
  the other is a remote.

To run the example, first build it:
```bash
npm run compile
```

And then start it:

```bash
npm run start
// or
node ./scripts/bin/index.js
```

You should now see that the connector is up and running and it should print the url for the descriptor, something like:

```bash
info: ===== ROUTES START =====
info: # Files:
info:     /examples.local.js
info: # Commands:
info:     GET : /samples.echo/remote/echo
info: # Root modules:
info:     /samples.echo.json
info: ====== ROUTES END ======
info: server started. listening on localhost:3333
info: connector started
```


Now open the fugazi client (http://fugazi.io) or a locally running instance and load the module:  
```fugazi
load module from "http://localhost:3333/samples.echo.json"
```

After the module was loaded try it:  
```fugazi
echo remote hey
```
Or 
```fugazi
echo local "how are you?"
```

## Using as a package
The connector is created using builders in such a way that building can be chained.  
Example of usage (how the echo example is created):
```javascript
import * as connector from "@fugazi/connector";

const CONNECTOR = new connector.ConnectorBuilder()
	.server()
		.cors(true) // not needed, it's the default
		.parent()
	.module("samples.echo")
		.descriptor({
			title: "Echo example",
			description: "Example of a (echo) remote command using the node connector"
		})
		.module("remote")
			.descriptor({
				title: "Remote echo module",
			})
			.command("echo", {
				title: "Echo command",
				returns: "string",
				syntax: "remote echo (str string)"
			})
			.handler((request) => {
				return { data: request.data("str") };
			})
	.build();

CONNECTOR.start().then(() => {
	CONNECTOR.logger.info("connector started");
});
```

## Using when cloning
1. Clone this repo (let's say that it was cloned to `/CONNECTOR/PATH`)
2. Install dependencies: `/CONNECTOR/PATH > npm install`
3. Compile the scripts:  
`/CONNECTOR/PATH > ./node_modules/typescript/bin/tsc -p ./scripts`

The rest is the same as the sections above.