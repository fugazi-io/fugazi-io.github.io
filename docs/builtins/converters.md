# Built-in Converters

Useful converters.

## String to boolean
Converts a value of type [string](builtins/types?id=string) to a value of type [boolean](builtins/types?id=boolean).

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"returns": "boolean",
	"convert": {
		"from": "string"
	},
	...
}
```
Equivalent to:
```json
{
	"name": "mycommand",
	"returns": "boolean",
	"convert": {
		"from": "string",
		"converter": "string2boolean"
	},
	...
}
```

----

## String to number
Converts a value of type [string](builtins/types?id=string) to a value of type [number](builtins/types?id=number).

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"returns": "number",
	"convert": {
		"from": "string"
	},
	...
}
```
Equivalent to:
```json
{
	"name": "mycommand",
	"returns": "number",
	"convert": {
		"from": "string",
		"converter": "string2number"
	},
	...
}
```