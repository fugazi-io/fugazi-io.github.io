# Package @fugazi/connector/server
## Package Types
### HttpMethod
```typescript
type HttpMethod = "get" | "GET" | "put" | "PUT" | "post" | "POST" | "delete" | "DELETE";
```
A string which matches one of the values listed above which represents an [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods).

### RequestDataGetter
```typescript
type RequestDataGetter<T> = {
	(name: string): T;
	has(name: string): boolean;
}
```
A data getter function which also has a _has_ function.

### RequestData
```typescript
type RequestData = RequestDataGetter<string | any> & {
	body: RequestDataGetter<any>;
	path: RequestDataGetter<string>;
	search: RequestDataGetter<string>;
}
```
A object which contains the data sent along with the request.

### Request
```typescript
type Request = {
	path: string;
	data: RequestData;
	headers: Map<string, string>;
}
```
An object representing an http request.

### ResponseStatus
```typescript
enum ResponseStatus {
	Success,
	Failure
}
```

### Response
```typescript
type Response = {
	headers?: { [name: string]: string } | Map<string, string>;
	status?: ResponseStatus;
	data?: any;
}
```
An object representing an http response.

### CommandHandler
```typescript
type CommandHandler = (request: Request) => Response | Promise<Response>;
```
A callback function which is invoked when a request is made for a specific command.  
This handler receives the request object (which contains the data, headers) and needs to return a [Response](server?id=response) 
object (synchronous) or a _Promise_ for such an object.

### CommandEndpoint
```typescript
type CommandEndpoint = {
	path: string;
	method: HttpMethod;
	handler: CommandHandler;
}
```
An object representing all of the information needed to create and serve a command request.

----

## Package Classes
### Server
An immutable object for interacting with the http server that is created for serving the different descriptors, commands 
and other needed files.

```typescript
class Server {
	public start(): Promise<void>;
	public stop(): Promise<void>;
}
```

#### Methods
##### start()
_public start(): Promise&lt;void&gt;_  
Starts the server.  
Shouldn't be used directly, instead use the [Connector.start](connector?id=start) method.

**Returns:**  
A promise to when the server has started or failed starting

##### stop()
_public stop(): Promise&lt;void&gt;_  
Stops the server.  
Shouldn't be used directly, instead use the [Connector.stop](connector?id=stop) method.

**Returns:**  
A promise to when the server has stopped or failed stopping

----

### ServerBuilder
A class for creating instances of [Server](?id=server).  
The builder pattern is used for most methods, so chaining can be used.

```typescript
class ServerBuilder {
	public static readonly DEFAULT_HOST = "localhost";
	public static readonly DEFAULT_PORT = 3333;
	
	public logger(logger: winston.LoggerInstance): this;
	public port(port: number): this;
	public host(host: string): this;
	public cors(enabled: boolean): this;
	public cors(options: Cors.Options): this;
	public proxy(enabled: boolean): this;
	public file(urlPath: string, filePath: string): this;
	public module(descriptor: Module): this;
	public command(command: CommandEndpoint): this;
	public parent(): ConnectorBuilder;
	public build(): Server;
	public getOrigin(): string;
	public getUrlFor(relativePath: string): string;
	public getProxy(): string | null;
}
```

#### Static Members
##### DEFAULT_HOST
<em>public static readonly DEFAULT_HOST = "localhost"</em>  
The default host which will be used to bind the connector to in case a different host is provided.

##### DEFAULT_PORT
<em>public static readonly DEFAULT_PORT = 3333</em>  
The default port which will be used to bind the connector to in case a different port is provided.

#### Methods
##### logger(logger)
_public logger(logger: winston.LoggerInstance): this_  
Sets the logger instance for the server instance.  
There's no need to call this method, it is automatically set by the [ConnectorBuilder](connector?id=connectorbuilder)  

**Parameters:**  
logger - the winston logger instance to be used

**Returns:**  
_this_ instance to be used for chaining  

**See also:**  
 - GitHub repo: [https://github.com/winstonjs/winston](https://github.com/winstonjs/winston)
 - Typescript definitions: [https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/winston/index.d.ts](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/winston/index.d.ts)

##### port(port)
_public port(port: number): this_  
Sets the port number to which the connector server will be bound to.  
If this method isn't used then the [ServerBuilder.DEFAULT_PORT](?id=default_port) will be used.

**Parameters:**  
port - the port number to bind to

**Returns:**  
_this_ instance to be used for chaining  

##### host(host)
_public host(host: string): this_  
Sets the host name to which the connector server will be bound to.  
If this method isn't used then the [ServerBuilder.DEFAULT_HOST](?id=default_host) will be used.

**Parameters:**  
host - the host name to bind to

**Returns:**  
_this_ instance to be used for chaining  

##### cors(enabled)
_cors(enabled: boolean): this_  
Control whether or not [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) is used.  

**Parameters:**  
enabled - should CORS be enabled or not

**Returns:**  
_this_ instance to be used for chaining  

##### cors(options)
_cors(options: Cors.Options): this_  
Enables CORS with the passed in [options](https://github.com/koajs/cors)  

**Parameters:**  
options - the CORS options to be used

**Returns:**  
_this_ instance to be used for chaining  

##### proxy(enabled)
_proxy(enabled: boolean): this_  
Control whether or not a proxy frame should be used instead of CORS.  

**Parameters:**  
enabled - should a proxy frame be used

**Returns:**  
_this_ instance to be used for chaining  

##### file(urlPath, filePath)
_file(urlPath: string, filePath: string): this_  
Adds a file to be served by the server.  
In most cases this method will be invoked by the [ModuleBuilder](components?id=modulebuilder) and should not be called manually.

**Parameters:**  
urlPath - the endpoint path to serve the file  
filePath - the filesystem path for the file

**Returns:**  
_this_ instance to be used for chaining  

##### module(descriptor)
_module(descriptor: [Module](../../#descriptors?id=module-descriptor ":target=_self"))_  
Adds a module descriptor to be served by the server.  
This method will be invoked by the [ModuleBuilder](components?id=modulebuilder) and should not be called manually.

**Parameters:**  
descriptor - the descriptor for the module  

**Returns:**  
_this_ instance to be used for chaining  

##### command(endpoint)
_command(command: [CommandEndpoint](server?id=commandendpoint))_  
Adds a command to be served by the server.  
This method will be invoked by the [ModuleBuilder](components?id=modulebuilder) and should not be called manually.

**Parameters:**  
command - the command to be served  

**Returns:**  
_this_ instance to be used for chaining  

##### parent()
_public parent(): [ConnectorBuilder](connector?id=connectorbuilder)_  

**Returns:**  
A reference to the connector builder

##### build()
_public build(): [Server](?id=server)_  
Builds an instance of the winston logger.  
Should not be called directly, it happens automatically when calling [ConnectorBuilder.build](connector?id=build).

**Returns:**  
An instance of a server

##### getOrigin()
_getOrigin(): string_  

**Returns:**  
The base url for the server  

##### getUrlFor(relativePath)
_getUrlFor(relativePath: string): string_  

**Parameters:**  
relativePath - the path relative to the base url of the server

**Returns:**  
The full url string for the passed path along with the base url for the server  

##### getProxy()
_getProxy(): string | null_  

**Returns:**  
The url for the proxy frame, or _null_ if proxy isn't enabled