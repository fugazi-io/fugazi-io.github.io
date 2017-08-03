# Descriptors

Descriptors are the way to explain to the client what your service/application exposes, and they come in 
the form of a json object. Each descriptor describes one component (but a module also contain inner descriptors).

## Basic descriptor
all descriptors share the following properties:
```typescript
interface Descriptor {
	name: string;
	title?: string;
	description?: string;
}
```

### Fields
 * **name:** A required string which acts as the id for the component.  
 The name must be unique for the collection of components of the same type in its parent.  
 There are currently no restrictions on the name of the module (except for dot and space), but it might change 
 in the future, so try to stick to alpha/alpha-numeric names.

 * **title:** An optional human readable string with a short description of the component.  
 There are no restrictions on the title, but avoid using new lines and try to keep it up to 150 chars.  
 When missing it will be substituted with the _name_ field with the first char capitalized.
 
 * **description:** An optional string to describe in more lengths what this component is all about.

---

## Type Descriptor
Describes a [type component](components?id=types)
```typescript
type Definition = string | { [key: string]: Definition };

interface TypeDescriptor extends Descriptor {
	type: Definition;
}

interface StructDescriptor extends TypeDescriptor {
	base?: string;
}
```

### Fields
 * **TypeDescriptor.type:** A required type definition.  
 The definition can be one of the two:
  * A string starting with the base type and a list of constraints
  * An object describing a struct type, where the keys are the name of the fields and the values are the 
  types of the fields.
 * **StructDescriptor.base:** An optional type name (must be another struct) which this type will extend.
 
### Examples
```json
{
	"name": "mystring",
	"type": "string"
}
```
```json
{
	"name": "integer",
	"type": "number[numbers.integer]"
}
```
```json
{
	"name": "user",
	"type": {
		"id": "string[strings.regex '^\\w+$', strings.between 5 10]",
		"username": "string[strings.regex '^[\\w_.]+$', strings.between 3 25]",
		"birthYear": "number[numbers.integer, numbers.between 1950 2000]"
	}
}
```

---

## Constraint Descriptor
Describes a [constraint component](components?id=constraints)
```typescript
type BoundConstraintValidator = (value: any) => boolean;
type UnboundConstraintValidator = (...args: any[]) => BoundConstraintValidator;

interface ConstraintDescriptor extends Descriptor {
	types: string[];
	params?: string[];
	validator: UnboundConstraintValidator;
}
```

### Fields
 * **types:** A required array of type names this constraint can works with
 * **params:** An optional array of parameter names which the validator expects
 * **validator:** A required javascript function which can expect arguments, and returns another function 
 which receives a value and should return a boolean indicating whether the value passes the validation

### Examples
```typescript
{
	name: "integer",
	types: ["number"],
	validator: function() {
		return function (value: number): boolean {
			return Number.isInteger(value);
		}
	}
}
```
```typescript
{
	name: "between",
	types: ["number"],
	params: ["min", "max"],
	validator: function (min: number, max: number) {
		return function (value: number): boolean {
			return value >= min && value <= max;
		}
	}
}
```

---

## Converter Descriptor
Describes a [converter component](components?id=converters)
```typescript
interface ConverterDescriptor extends Descriptor {
	input: string;
	output: string;
	converter: (input: any) => any;
}
```

### Fields
 * **input:** A required string with the name of the type to convert from
 * **output:** A required string with the name of the type to convert to
 * **converter:** A required function that takes an input of type _input_ and returns a value of type 
 _output_
 
### Examples
```typescript
{
	name: "string2user",
	input: "string",
	output: "user",
	converter: (str: string): { id: string, username: string } => {
		return JSON.parse(str);
	}
}
```

---

## Command Descriptor
Describes a [command component](components?id=commands)
```typescript
interface CommandDescriptor extends Descriptor {
	returns: TypeDefinition;
	convert?: {
		from: string;
		converter?: string;
	};
	syntax: string | string[];
	async?: boolean;
}

enum ResultStatus {
	Success,
	Failure
}

interface Result {
	status: ResultStatus;
}

interface SuccessResult extends Result {
	value?: any;
}

interface FailResult extends Result {
	error: string;
}

interface LocalCommandDescriptor extends CommandDescriptor {
	parametersForm?: "list" | "arguments" | "map" | "struct";
	handler: (context: ModuleContext, ...params: any[]) => Result | Promise<Result>;
}

interface RemoteCommandDescriptor extends CommandDescriptor {
	handler: {
		endpoint: string;
		method?: string;
	};
}
```

### Fields
 * **CommandDescriptor.returns:** A required string with the name of the type of the return value
 * **CommandDescriptor.convert:** Optional object which describes which converter is to be used for the return 
 value. It includes the type of the actual return type and the name of the converter to be used.  
 The converter needs to convert between `convert.from` type to the `returns` type.
 * **CommandDescriptor.syntax:** A required string or array of strings of the syntax rules for this command
 * **CommandDescriptor.async:** An optional boolean saying if this command is asynchronous or not, default to _false_
 * **LocalCommandDescriptor.parametersForm:** An optional string to indicate how the parameters will be passed 
 to the javascript handler function (default is _arguments_):
  * _list_: the handler will receive the params as an array
  * _arguments_: the handler will receive the params as regular function arguments
  * _map_: the handler will receive the params as a `Map` instance (key/value)
  * _struct_: the handler will receive the params as a javascript object (key/value)
 * **LocalCommandDescriptor.handler:** A required function which will be invoked when the command is executed.  
 The function should expect a context (advanced usage and undocumented yet) and then the params in the form 
 selected in _parametersForm_. The function needs to return either a _Result_ or a _Promise_ to one.
 * **RemoteCommandDescriptor.handler:** A required object with the remote handler info.  
 The _endpoint_ is a required string with the url path for the command.  
 The _method_ is the http method to be used, default to _GET_.
 
### Examples
```typescript
{
	name: "add",
	returns: "number",
	parametersForm: "map",
	syntax: [
		"(a number) + (b number)",
		"add (a number) (b number)",
		"add (numbers list<number>)"
	],
	handler: function(context, params): number {
		if (params.has("a") && params.has("b")) {
			return params.get("a") + params.get("b");
		}

		if (params.has("numbers")) {
			return params.get("numbers").reduce((previousValue, currentValue) => previousValue + currentValue);
		}
	}
}
```
```typescript
{
	title: "factorial",
	returns: "number",
	parametersForm: "arguments",
	syntax: [
		"(a integer)!",
		"factorial of (a integer)"
	],
	handler: function(context, a): number {
		let i = 1,
			result = 1;

		while (i++ < a) {
			result *= i;
		}

		return result;
	}
}
```
```json
{
	"name": "echo",
	"returns": "string",
	"syntax": "echo (value string)",
	"handler": {
		"endpoint": "/echo/{ value }"
	}
}
```

---

## Module Descriptor
Describes a [module component](components?id=module)
```typescript
interface SourceRemoteDescriptor {
	base?: string;
	proxy?: string;
	origin: string;
}

interface RelativeRemoteDescriptor {
	path: string;
}

type InnerComponentsCollection<T> = Array<string | T> | { [name: string]: T };

interface ModuleDescriptor extends Descriptor {
	remote?: SourceRemoteDescriptor | RelativeRemoteDescriptor;
	modules?: InnerComponentsCollection<ModuleDescriptor>;
	types?: InnerComponentsCollection<TypeDescriptor>;
	commands?: InnerComponentsCollection<CommandDescriptor>;
	converters?: InnerComponentsCollection<ConverterDescriptor>;
	constraints?: InnerComponentsCollection<ConstraintDescriptor>;
}
```

### Fields
 * **remote:** An optional object defining the remote configuration, only needed for remote modules.  
 An inner remote module just need to define the url path part (which will be added to the base).  
 The root remote module should define:
  * _base_: Optional string for the base url path to append to all urls
  * _proxy_: Optional boolean stating whether or not to use a proxy frame to execute the commands
  * _origin_: A required string which is the base url for all the commands urls
 * **modules:** An optional object or array with inner modules.  
 In case of an object the keys are the name of the inner modules and the values are the rest of the needed 
 properties for the modules.  In case of an array, each item can either be a url string for another module to 
 be loaded or an object with the module properties.
 * **types:** An optional object or array with inner types.
 * **commands:** An optional object or array with inner types.
 * **converters:** An optional object or array with inner types.
 * **constraints:** An optional object or array with inner types.

### Examples
```javascript
// http://localhost:3333/examples.local.js
(function () {
	fugazi.components.modules.descriptor.loaded({
		name: "samples.echo.local",
		title: "Simple local command examples",
		commands: {
			"echo": {
				title: "Echo command",
				returns: "string",
				parametersForm: "arguments",
				syntax: "local echo (str string)",
				handler: function (context, str) {
					return str;
				}
			}
		}
	});
})();
```
```json
{
	"name": "samples.echo",
	"title": "Echo example",
	"description": "Example of a (echo) remote and local commands using the node connector",
	"remote":{
		"origin":"http://localhost:3333"
	},
	"modules": [
		{
			"name": "remote",
			"title": "Remote echo module",
			"commands": {
				"echo": {
					"name": "echo",
					"title": "Echo command",
					"returns": "string",
					"syntax": "remote echo (str string)",
					"handler": {
						"method": "GET",
						"endpoint": "/samples.echo/remote/echo"
					}
				}
			}
		},
		"http://localhost:3333/examples.local.js"
	]
}
```