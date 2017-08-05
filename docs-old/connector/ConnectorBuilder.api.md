# ConnectorBuilder class

Manages the configuration and the creation of [Connector](connector/Connector.api.md) instances.

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

## Constructors
#### constructor()
_public constructor()_  
Default constructor  

## Methods
#### server()
_public server(): [ServerBuilder](connector/ServerBuilder.api.md)_  

**Returns:**  
The instance for the [ServerBuilder](connector/ServerBuilder.api.md) which can be used to configure the 
instance of the [Server](connector/Server.api.md) that is returned by [Connector.server](connector/Connector.api?id=server-server).

#### logger()
_public logger(): [LoggingBuilder](connector/LoggingBuilder.api.md)_  

**Returns:**  
The builder to be used to configure the instance of the logger that is returned by [Connector.logger](connector/Connector.api?id=logger-winstonloggerinstance).

#### module(name)
_public module(obj: string): [RootModuleBuilder]()_  
Creates a new [RootModuleBuilder]() by it's name to be used for creating a [RootModule]().  

**Parameters:**  
obj - the name of the module to be created

#### module(descriptor)
_public module(obj: [descriptors.Named]()<Partial<[descriptors.Module]()>>): [RootModuleBuilder]()_  
Creates a new [RootModuleBuilder]() by it's descriptor to be used for creating a [RootModule]().  

**Parameters:**  
obj - the descriptor of the module to be created, can be partial but must include the name

#### build()
_public build(): [Connector](connector/Connector.api.md)_  
Builds the connector  

**Returns:**  
The instance of the connector build based on how this connector was configured