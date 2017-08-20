# Package @fugazi/connector

## Package Constants
### Version
_const VERSION: string_  

The connector package version, same as the one in the [package.json](https://github.com/fugazi-io/connector.node/blob/master/package.json) file.

----

## Package Classes
### Connector
An immutable facade to the entire functionality which the package offers.  
An instance can only be created using the [ConnectorBuilder](?id=connectorbuilder).

```typescript
declare class Connector {
	public readonly server: Server;
	public readonly logger: winston.LoggerInstance;
	public start(): Promise<void>;
	public stop(): Promise<void>;
}
```

#### Members
##### server
_public readonly server: [Server](connector/Server.api.md)_  
A readonly reference to the server instance which is used by this.

**See also:**  
[ServerBuilder](server?id=serverbuilder)

##### logger
_public readonly logger: winston.LoggerInstance_  
A readonly reference to the winston `LoggerInstance` instance which is used by this.  

**See also:**  
 - GitHub repo: [https://github.com/winstonjs/winston](https://github.com/winstonjs/winston)
 - Typescript definitions: [https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/winston/index.d.ts](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/winston/index.d.ts)

#### Methods
##### start()
_public start(): Promise&lt;void&gt;_  
Starts the server.

**Returns:**  
A promise to when the server has started or failed starting

##### stop()
_public stop(): Promise&lt;void&gt;_  
Stops the server.

**Returns:**  
A promise to when the server has stopped or failed stopping

----

### ConnectorBuilder
Manages the configuration and the creation of [Connector](?id=connector) instances.

```typescript
declare class ConnectorBuilder {
	public constructor();
	public server(): ServerBuilder;
	public logger(): LoggingBuilder;
	public module(obj: string): RootModuleBuilder;
	public module(obj: descriptors.Named<Partial<descriptors.Module>>): RootModuleBuilder;
	public build(): Connector;
}
```

#### Constructors
##### constructor()
_public constructor()_  
Default constructor  

#### Methods
##### server()
_public server(): [ServerBuilder](connector/ServerBuilder.api.md)_  

**Returns:**  
The instance for the [ServerBuilder](connector/ServerBuilder.api.md) which can be used to configure the 
instance of the [Server](connector/Server.api.md) that is returned by [Connector.server](connector/Connector.api?id=server-server).

##### logger()
_public logger(): [LoggingBuilder](connector/LoggingBuilder.api.md)_  

**Returns:**  
The builder to be used to configure the instance of the logger that is returned by [Connector.logger](connector/Connector.api?id=logger-winstonloggerinstance).

##### module(name)
_public module(obj: string): [RootModuleBuilder](components?id=rootmodulebuilder)_  
Creates a new [RootModuleBuilder](components?id=rootmodulebuilder) by it's name to be used for creating a [RootModule](../../#descriptors?id=module-descriptor ":target=_self").  

**Parameters:**  
obj - the name of the module to be created

##### module(descriptor)
_public module(obj: Partial&lt;[Module](../../#descriptors?id=module-descriptor ":target=_self")&gt;): [RootModuleBuilder](components?id=rootmodulebuilder)_  
Creates a new [RootModuleBuilder](components?id=rootmodulebuilder) by it's descriptor to be used for creating a [RootModule](../../#descriptors?id=module-descriptor ":target=_self").  

**Parameters:**  
obj - the descriptor of the module to be created, can be partial but must include the name

##### build()
_public build(): [Connector](connector/Connector.api.md)_  
Builds the connector  

**Returns:**  
The instance of the connector build based on how this connector was configured
