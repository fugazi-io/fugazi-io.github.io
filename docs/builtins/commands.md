# Built-in Commands

Useful commands.

### Version
Outputs the version of the client.  

Syntax:
```
version
```

----

### Help
Prints explanation for a specific subject.

Syntax:
```
help
help vars
help types
help syntax
help search
help history
help commands
help converters
help constraints
help suggestions
```

----

### Manual
Prints the component information.

Syntax:
```
man command-name
man component-path
```

Examples:
```fugazi
man load
```
```fugazi
man "io.fugazi.components"
```

----

### Set
Stores a value into a variable.

Syntax:
```
VAR_NAME = VALUE
set VAR_NAME = VALUE
```

Examples:
```fugazi
a = 3
```
```fugazi
set a = [1, 2, 3]
```

----

### Echo
Outputs the passed value.  

Syntax:
```
echo VALUE
```

Examples:
```fugzi
echo 4
```
```fugzi
echo [1, 2, 3]
```
```fugzi
a = { key: "value", "key2": 4 }
echo $a
```

----

### Clear
Clears the output panel.

Syntax:
```
clear
```

----

### Extract
Returns an inner value inside a compound value.

Syntax:
```
(value map) . (index string)
(value map) (index list<string>)
extract (index string) from (value map)
(value list) (index list<number[numbers.integer]>)
extract (index number[numbers.integer]) from (value list)
```

Examples:
```fugzi
list = [1, 2, 3]
extract 1 from $list
$list [1]
```
```fugzi
map = { key1: value1, key2: value2, key3: { inner1: "a value" } }
extract key1 from $map
$map [key3, inner1]
$map . key2
```

----

## Components commands
### Load
Loads a [module](/components?id=modules) from a url.  
Urls must end with either _.json_ or _.js_.

Syntax:
```
load module from URL
```

Examples:
```fugazi
load module from "http://fugazi.io/modules/scripts/bin/math.js"
```

----

### List
Outputs the list of different component types in different modules.

Syntax:
```
list modules
list modules in MODULE_PATH
list types in MODULE_PATH
list commands in MODULE_PATH
list converters in MODULE_PATH
list constraints in MODULE_PATH
```

Examples:

```fugazi
list modules
```
```fugazi
list modules in "io.fugazi"
```
```fugazi
list types in "io.fugazi.core"
```

----

## Json commands
### Parse
Returns an the json value equivalent of the passed string.

Syntax:
```
json parse JSON_STRING
jsonParse JSON_STRING
```

Examples:

```fugazi
json parse '{ "key1": "value1", "key2": 4 }'
```
```fugazi
jsonParse '[1, 2, 3, "string"]'
```

----

### Stringify
Returns a json string representation of the passed value.

Syntax:
```
json stringify VALUE
jsonStringify VALUE
```

Examples:

```fugazi
json stringify { "key1": "value1", "key2": 4 }
```
```fugazi
jsonStringify [1, 2, 3, "string"]
```

----

## Net commands
### Http
Make an http request.

Syntax:
```
http URL METHOD
http URL METHOD DATA
http URL METHOD DATA HEADERS
http URL METHOD DATA TIMEOUT
http URL METHOD DATA CONTENT_TYPE
http URL METHOD DATA HEADERS TIMEOUT
http URL METHOD DATA CONTENT_TYPE HEADERS
http URL METHOD DATA CONTENT_TYPE TIMEOUT
http URL METHOD DATA CONTENT_TYPE HEADERS TIMEOUT
```

Examples:

```fugazi
http "https://httpbin.org/get" GET
```

----

### Get
Make an http GET request.   
Same as calling: http URL get.

Syntax:
```
get URL
get URL DATA
get URL DATA HEADERS
get URL DATA TIMEOUT
get URL DATA CONTENT_TYPE
get URL DATA HEADERS TIMEOUT
get URL DATA CONTENT_TYPE HEADERS
get URL DATA CONTENT_TYPE TIMEOUT
get URL DATA CONTENT_TYPE HEADERS TIMEOUT
```

Examples:

```fugazi
get "https://httpbin.org/get?key=value"
```
```fugazi
get "https://httpbin.org/get" { key: value }
```
```fugazi
get "https://httpbin.org/get" { key: value } { "X-MY-HEADER": "a header" }
```

----

### Post
Make an http Post request.   
Same as calling: http URL post.

Syntax:
```
post URL
post URL DATA
post URL DATA HEADERS
post URL DATA TIMEOUT
post URL DATA CONTENT_TYPE
post URL DATA HEADERS TIMEOUT
post URL DATA CONTENT_TYPE HEADERS
post URL DATA CONTENT_TYPE TIMEOUT
post URL DATA CONTENT_TYPE HEADERS TIMEOUT
```

Examples:

```fugazi
post "https://httpbin.org/post" { key1: value1 }
```

----

### Delete
Make an http DELETE request.   
Same as calling: http URL delete.

Syntax:
```
delete URL
delete URL DATA
delete URL DATA HEADERS
delete URL DATA TIMEOUT
delete URL DATA CONTENT_TYPE
delete URL DATA HEADERS TIMEOUT
delete URL DATA CONTENT_TYPE HEADERS
delete URL DATA CONTENT_TYPE TIMEOUT
delete URL DATA CONTENT_TYPE HEADERS TIMEOUT
```

Examples:

```fugazi
get "https://httpbin.org/get?key=value"
```
```fugazi
get "https://httpbin.org/get" { key: value }
```
```fugazi
get "https://httpbin.org/get" { key: value } { "X-MY-HEADER": "a header" }
```