# LoggingBuilder class

Manages the configuration and the creation of the _winston.LoggerOptions_ instance.  

```typescript
declare class LoggingBuilder {
	public constructor(parent: ConnectorBuilder);
	public parent(): ConnectorBuilder;
	public consoleLevel(level: LoggingLevel): this;
	public filePath(path: string): this;
	public fileLevel(level: LoggingLevel): this;
	public options(options: winston.LoggerOptions): this;
	public build(): winston.LoggerInstance;
}
```

## Static fields
#### DEFAULT_LEVEL
<em>public static readonly DEFAULT_LEVEL = "info"</em>  
The default minimum level for logging which will be used if not set.

## Constructors
#### constructor(parent)
_public constructor(parent: ConnectorBuilder)_  
Shouldn't be called directly, instead use the [ConnectorBuilder.logger](connector/ConnectorBuilder.api?id=logger) method to 
get a reference to an instance.

## Methods
#### parent()
_public parent(): [ConnectorBuilder](connector/ConnectorBuilder.api)_  

**Returns:**  
A reference to the connector builder

#### consoleLevel(level)
_public consoleLevel(level: [LoggingLevel](connector/API?id=logginglevel)): this_  
Sets the minimum level for console logging.  
Default is set to [DEFAULT_LEVEL](connector/LoggingBuilder.api?id=default_level).

**Parameters:**  
level - the minimum level for console logging

**Returns:**  
The instance of _this_, used for chaining

#### filePath(path)
_public filePath(path: string): this_  
Sets path for the file to which the logging will be saved  
If a path isn't set then logging to a file is disabled.

**Parameters:**  
path - the filesystem path for a file to be written into

**Returns:**  
The instance of _this_, used for chaining

#### fileLevel(level)
_public fileLevel(level: [LoggingLevel](connector/API?id=logginglevel)): this_  
Sets the minimum level for file logging.  
Default is set to [DEFAULT_LEVEL](connector/LoggingBuilder.api?id=default_level).

**Parameters:**  
level - the minimum level for console logging

**Returns:**  
The instance of _this_, used for chaining

#### options(options)
_public options(options: winston.LoggerOptions): this_  
Sets winston logger options.  

**Parameters:**  
options - the options object for winston

**Returns:**  
The instance of _this_, used for chaining

**See also:**  
 - GitHub repo: [https://github.com/winstonjs/winston](https://github.com/winstonjs/winston)
 - Typescript definitions: [https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/winston/index.d.ts](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/winston/index.d.ts)

#### build()
_public build(): winston.LoggerInstance_  
Builds an instance of the winston logger.  
**Should not** be called directly, it happens automatically when calling [ConnectorBuilder.build](connector/ConnectorBuilder.api?id=build).