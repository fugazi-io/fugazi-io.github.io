# Package @fugazi/connector/components
## Package Classes
### ComponentBuilder
The base builder for all [component descriptors](../../#descriptors ":target=_self").  

```typescript
abstract class ComponentBuilder<C extends Component> {
	public parent(): ModuleBuilder;
	public name(name: string): this;
	public title(title: string): this;
	public description(description: string): this;
	public descriptor(descriptor: C): this;
	public path(): string[];
	public build(): C;
}
```

#### Methods
##### parent()
_public parent(): [ModuleBuilder](components?id=modulebuilder)_  

**Returns:**  
A reference to the connector builder

##### name(name)
_public name(name: string): this_  
Sets the name for the component.  

**Parameters:**  
name - the name

**Returns:**  
_this_ instance to be used for chaining  

##### title(title)
_public title(title: string): this_  
Sets the title for the component.  

**Parameters:**  
title - the title

**Returns:**  
_this_ instance to be used for chaining  

##### description(description)
_public description(description: string): this_  
Sets the description for the component.  

**Parameters:**  
description - the description

**Returns:**  
_this_ instance to be used for chaining  

##### descriptor(descriptor)
_public description(descriptor: C): this_  
Sets the properties in the descriptor.  
This method is overridden by extended classes so that all properties of the generic descriptor can be used.

**Parameters:**  
descriptor - the descriptor for the component

**Returns:**  
_this_ instance to be used for chaining  

----

### TypeBuilder
Creates a [type descriptor](../../#descriptors?id=type-descriptor ":target=_self").  
Instances should be created using the [ModuleBuilder.type](components?id=typename) method.  

```typescript
class TypeBuilder extends ComponentBuilder<Type> {
	public type(value: string | TypeDefinition): this;
}
```

#### Methods
##### type(type)
<em>public type(value: string | [TypeDefinition](../../#descriptors?id=type-descriptor ":target=_self")): this</em>  
Sets the type definition, can be either a string (i.e.: `"number[numbers.min 10]"`) or: 
```javascript
{
	name: "MyType",
	type: {
		fname: "string",
		lname: "string"
	}
}
```  

**Parameters:**  
value - the type definition  

**Returns:**  
_this_ instance to be used for chaining  

----

### RemoteCommandBuilder
Creates a [remote command descriptor](../../#descriptors?id=command-descriptor ":target=_self").  
Instances should be created using the [ModuleBuilder.command](components?id=commanddescriptor-handler) method.  

```typescript
class RemoteCommandBuilder extends ComponentBuilder<RemoteCommand> {
	public syntax(...rules: string[]): this;
	public endpoint(path: string): this;
	public method(value: HttpMethod): this;
	public handler(fn: CommandHandler): this;
	public returns(type: TypeDefinition): this;
}
```

#### Methods
##### syntax(rules)
_public syntax(...rules: string[]): this_  
Add syntax rules for the command.

**Parameters:**  
rules - one or more rules  

**Returns:**  
_this_ instance to be used for chaining  

##### endpoint(path)
_public endpoint(path: string): this_  
Sets the endpoint path for this command.  
It's not necessary to manually set this, a path will automatically be created.

**Parameters:**  
path - the path for the url  

**Returns:**  
_this_ instance to be used for chaining  

##### method(method)
_public method(value: [HttpMethod](server?id=httpmethod)): this_  
Set the http method that this command expects, default is _GET_.

**Parameters:**  
value - the method to be used  

**Returns:**  
_this_ instance to be used for chaining  

##### handler(fn)
_public handler(fn: [CommandHandler](server?id=commandhandler)): this_  
Set the callback function to be invoked when a request is made to this command.

**Parameters:**  
value - the method to be used  

**Returns:**  
_this_ instance to be used for chaining  

##### returns(type)
<em>public returns(type: [TypeDefinition](../../#descriptors?id=type-descriptor ":target=_self")): this</em>  
Set the return type which the command returns, default is _"void"_.

**Parameters:**  
type - the type of the result value  

**Returns:**  
_this_ instance to be used for chaining  

----

### ModuleBuilder
Base builder for [inner](components?id=innermodulebuilder) and [root](components?id=rootmodulebuilder) modules.  
As modules aggregate other components, the module builders have methods for creating all types of descriptors.

```typescript
class ModuleBuilder<C extends Module> extends ComponentBuilder<Module> {
	public type(name: string): TypeBuilder;
	public type(descriptor: Type): this;
	public type(name: string, descriptor: Type): this;
	
	public types(file: string): this;
	public types(endpoint: string, file: string): this;
	
	public command(descriptor: RemoteCommand, handler?: CommandHandler): this;
	public command(name: string, descriptor?: Partial<RemoteCommand>, handler?: CommandHandler): RemoteCommandBuilder;
	
	public commands(file: string): this;
	public commands(endpoint: string, file: string): this;
	
	public converters(file: string): this;
	public converters(endpoint: string, file: string): this;
	
	public constraints(file: string): this;
	public constraints(endpoint: string, file: string): this;
	
	public module(name: string): ModuleBuilder;
	public module(descriptor: Module): this;
	public module(name: string, descriptor: Module): this;
	
	public modules(file: string): this;
	public modules(endpoint: string, file: string): this;
}
```

#### Methods
##### type(name)
_public type(name: string): [TypeBuilder](components?id=typebuilder)_  
Creates a new type builder which has no properties other than the passed in name, the rest of the properties should be 
added using the type builder which is returned.  
The new type builder is added to the list of types which this module will contain once built.  

**Parameters:**  
name - the name for the new type  

**Returns:**  
A new type builder  

##### type(descriptor)
<em>public type(descriptor: [Type](../../#descriptors?id=type-descriptor ":target=_self")): this</em>  
Adds a new type to this new module.  

**Parameters:**  
descriptor - the type descriptor to be added, **must** include a name  

**Returns:**  
_this_ instance to be used for chaining  

##### type(name, descriptor)
<em>public type(name: string, descriptor: [Type](../../#descriptors?id=type-descriptor ":target=_self")): this</em>  
Adds a new type to this new module.  
This method is just like the previous one, except that here the descriptor doesn't need to contain a name (if it does 
it will be overridden).

**Parameters:**  
name - the name of the added type  
descriptor - the type descriptor to be added  

**Returns:**  
_this_ instance to be used for chaining  

##### types(file)
_public types(file: string): this_  
Adds a descriptor file which contains type(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  
The endpoint path for the served file will be based on the name of the file.

**Parameters:**  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### types(endpoint, file)
_public types(endpoint: string, file: string): this_  
Adds a descriptor file which contains type(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  

**Parameters:**  
endpoint - the path for the url for the served file  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### command(descriptor, handler?)
<em>command(descriptor: [RemoteCommand](../../#descriptors?id=command-descriptor ":target=_self"), handler?: [CommandHandler](server?id=commandhandler)): this</em>  
Adds a remote command.

**Parameters:**  
descriptor - the **full** remote command descriptor  
handler - optional callback function to be invoked when a request is served  

**Returns:**  
_this_ instance to be used for chaining  

##### command(name, descriptor?, handler?)
<em>command(name: string, descriptor?: Partial<[RemoteCommand](../../#descriptors?id=command-descriptor ":target=_self")>, handler?: [CommandHandler](server?id=commandhandler)): [RemoteCommandBuilder](components?id=remotecommandbuilder)</em>  
Creates a new remote command builder with the passed in name and other properties which are included in the passed in 
descriptor.  
The new remote command builder is added to the list of remote commands which this module will contain once built.  

**Parameters:**  
name - the name of the new command  
descriptor - optional and partial remote command descriptor  
handler - optional callback function to be invoked when a request is served  

**Returns:**  
A new remote command builder  

##### commands(file)
_public commands(file: string): this_  
Adds a descriptor file which contains command(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  
The endpoint path for the served file will be based on the name of the file.

**Parameters:**  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### commands(endpoint, file)
_public commands(endpoint: string, file: string): this_  
Adds a descriptor file which contains command(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  

**Parameters:**  
endpoint - the path for the url for the served file  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### converters(file)
_public converters(file: string): this_  
Adds a descriptor file which contains converter(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  
The endpoint path for the served file will be based on the name of the file.

**Parameters:**  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### converters(endpoint, file)
_public converters(endpoint: string, file: string): this_  
Adds a descriptor file which contains converter(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  

**Parameters:**  
endpoint - the path for the url for the served file  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### constraints(file)
_public constraints(file: string): this_  
Adds a descriptor file which contains constraint(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  
The endpoint path for the served file will be based on the name of the file.

**Parameters:**  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### constraints(endpoint, file)
_public constraints(endpoint: string, file: string): this_  
Adds a descriptor file which contains constraint(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  

**Parameters:**  
endpoint - the path for the url for the served file  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### module(name)
_public module(name: string): [InnerModuleBuilder](components?id=innermodulebuilder)_  
Creates a new inner module builder which has no properties other than the passed in name, the rest of the properties should be 
added using the builder which is returned.  
The new inner module builder is added to the list of inner modules which this module will contain once built.  

**Parameters:**  
name - the name for the new inner module  

**Returns:**  
A new inner module builder  

##### module(descriptor)
<em>public module(descriptor: [Module](../../#descriptors?id=module-descriptor ":target=_self")): this</em>  
Adds a new inner module to this new module.  

**Parameters:**  
descriptor - the inner module descriptor to be added, **must** include a name  

**Returns:**  
_this_ instance to be used for chaining  

##### module(name, descriptor)
<em>public module(name: string, descriptor: [Module](../../#descriptors?id=module-descriptor ":target=_self")): this</em>  
Adds a new inner module to this new module.  
This method is just like the previous one, except that here the descriptor doesn't need to contain a name (if it does 
it will be overridden).

**Parameters:**  
name - the name of the added inner module  
descriptor - the inner module descriptor to be added  

**Returns:**  
_this_ instance to be used for chaining  

##### modules(file)
_public modules(file: string): this_  
Adds a descriptor file which contains module(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  
The endpoint path for the served file will be based on the name of the file.

**Parameters:**  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

##### modules(endpoint, file)
_public modules(endpoint: string, file: string): this_  
Adds a descriptor file which contains module(s) descriptors.  
The file will be served by the server, and a reference to it will be added to this module descriptor.  

**Parameters:**  
endpoint - the path for the url for the served file  
file - the filesystem path for the file  

**Returns:**  
_this_ instance to be used for chaining  

----

### InnerModuleBuilder
Creates a [inner module](../../#descriptors?id=module-descriptor ":target=_self").  
Instances should be created using the [ModuleBuilder.module](components?id=modulename) method.  

```typescript
class InnerModuleBuilder extends ModuleBuilder<Module> {
	public remote(path: string): this;
}
```

### Methods
##### remote(path)
_public remote(path: string): this_  
Sets a path part in the url for any commands in this module or inner modules.

**Parameters:**  
path - the path part  

**Returns:**  
_this_ instance to be used for chaining  

----

### RootModuleBuilder
Creates a [root module](../../#descriptors?id=module-descriptor ":target=_self") which can be used to create all 
the different (nested) components..  
Instances should be created using the [ModuleBuilder.module](components?id=modulename) method.  

```typescript
class RootModuleBuilder extends ModuleBuilder<RootModule> {
	public base(remoteBase: string): this;
}
```

### Methods
##### base(remoteBase)
_public base(remoteBase: string): this_  
Sets the base url path for the commands this module, or any inner modules, have.

**Parameters:**  
remoteBase - the base url path  

**Returns:**  
_this_ instance to be used for chaining  
