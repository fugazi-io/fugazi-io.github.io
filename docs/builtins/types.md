# Built-in Types

The core types which are the base for the fugazi type system, along with some useful types.

## Core types
### Void
Can be used as the returns type of a command if it does not return a value.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"returns": "void",
	...
}
```

----

### Any
Matches all values.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": "do something (myparam any)",
	...
}
```

##### Input examples
```fugazi
do something 3
```
```fugazi
do something "my string"
```

----

### Boolean
Matches true or false without quotes and case is ignored.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": "do something (myparam boolean)",
	...
}
```

##### Input examples
```fugazi
do something true
```
```fugazi
do something else FaLse
```

----

### Number
Matches any number (float, integer, positive, negative) without quotes.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": "do something (myparam number)",
	...
}
```

##### Input examples
```fugazi
do something 3
```
```fugazi
do something else -4.4
```

----

### String
Matches a quoted string or any single word that isn't a boolean or a number.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": "do something (myparam string)",
	...
}
```

##### Input examples
```fugazi
do something str
```
```fugazi
do something else "my string"
```

----

### List
Matches one or more values which can be of any other type.  
A list can be generic: `list<string>`, if the generic constraint isn't set _any_ is assumed.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": [
		"do something (myparam list)",
		"do something else (myparam list<number>)",
	],
	...
}
```

##### Input examples
```fugazi
do something [1, 2, 3]
```

----

### Map
Matches one or more key/values.  
The key can only be a string, but the value can be of any other type.  
A map can be generic: map<string>, if the generic constraint isn't set any is assumed.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": [
		"do something (myparam map)",
		"do something else (myparam map<boolean>)",
	],
	...
}
```

##### Input examples
```fugazi
do something { key1: value1, "key2": 5 }
```

----

## Numbers
### Integer
Matches only integer numbers.  
This type is an alias to `number[numbers.integer]`

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": [
		"do something (myparam numbers.integer)"
	],
	...
}
```

##### Input examples
```fugazi
do something 4
```

----

### Float
Matches only float numbers.  
This type is an alias to `number[numbers.float]`

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": [
		"do something (myparam numbers.float)"
	],
	...
}
```

##### Input examples
```fugazi
do something 3.3
```

----

## Net
### Url
Matches strings with a url pattern.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": [
		"do something (myparam net.url)"
	],
	...
}
```

##### Input examples
```fugazi
do something "http://www.example.com"
```

----

### Email
Matches strings with an email pattern.

##### Usage in descriptors
```json
{
	"name": "mycommand",
	"syntax": [
		"do something (myparam net.email)"
	],
	...
}
```

##### Input examples
```fugazi
do something "me@domain.com"
```