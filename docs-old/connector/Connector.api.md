# @fugazi/connector.Connector class

An immutable facade to the entire functionality which the package offers.  
An instance can only be created using the [ConnectorBuilder](connector/ConnectorBuilder.api.md).

```typescript
declare class Connector {
	public readonly server: Server;
	public readonly logger: winston.LoggerInstance;
	public start(): Promise<void>;
	public stop(): Promise<void>;
}
```

## Members
#### server
_public readonly server: [Server](connector/Server.api.md)_  
A readonly reference to the server instance which is used by this.

**See also:**  
[ServerBuilder]()

#### logger
_public readonly logger: winston.LoggerInstance_  
A readonly reference to the winston `LoggerInstance` instance which is used by this.  

**See also:**  
 - GitHub repo: [https://github.com/winstonjs/winston](https://github.com/winstonjs/winston)
 - Typescript definitions: [https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/winston/index.d.ts](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/winston/index.d.ts)

## Methods
#### start()
_public start(): Promise<void>_  
Starts the server.

**Returns:**  
A promise to when the server has started or failed starting

#### stop()
_public stop(): Promise<void>_  
Stops the server.

**Returns:**  
A promise to when the server has stopped or failed stopping