# Built-in Constraints

Useful constraints for creating custom new types.

## Core Constraints
### Enum
Checks that a value is included in a list of predetermined values.  
Applies to: [string](builtins/types?id=string).  
Syntax: `enum ...VALUES` or `enum ignoreCase ...VALUES`.

##### Usage in descriptors
```json
{
	"name": "myEnumType",
	"type": "string[enum ignoreCase one two three]"
}
```
```json
{
	"name": "myCommand",
	"syntax": "do something string[enum ONE TWO THREE]"
}
```

----

## Numbers
### Integer
Checks that a value is an integer.  
Applies to: [number](builtins/types?id=number).  
Syntax: `integer`.  
Also: [integer type](builtins/types?id=integer).

##### Usage in descriptors
```json
{
	"name": "myIntegerType",
	"type": "number[numbers.integer]"
}
```

----

### Float
Checks that a value is an float.  
Applies to: [number](builtins/types?id=number).  
Syntax: `float`.  
Also: [float type](builtins/types?id=float).

##### Usage in descriptors
```json
{
	"name": "myFloatType",
	"type": "number[numbers.float]"
}
```

----

### Minimum
Checks that a value is not below a specific threshold.  
Applies to: [number](builtins/types?id=number).  
Syntax: `min (num number)`.

##### Usage in descriptors
```json
{
	"name": "greaterThanFive",
	"type": "number[numbers.min 5]"
}
```

----

### Maximum
Checks that a value is up to a specific limit.  
Applies to: [number](builtins/types?id=number).  
Syntax: `max (num number)`.

##### Usage in descriptors
```json
{
	"name": "smallerThanTen",
	"type": "number[numbers.max 10]"
}
```

----

### Between
Checks that a value is between a specific range.  
Applies to: [number](builtins/types?id=number).  
Syntax: `between (min number) (max number)`.

##### Usage in descriptors
```json
{
	"name": "between5and10",
	"type": "number[numbers.between 5 10]"
}
```

----

### Positive
Checks that a value is equal or greater than 0.  
Applies to: [number](builtins/types?id=number).  
Syntax: `positive`.

##### Usage in descriptors
```json
{
	"name": "myPositiveNumber",
	"type": "number[numbers.positive]"
}
```

----

### Negative
Checks that a value is less than 0.  
Applies to: [number](builtins/types?id=number).  
Syntax: `negative`.

##### Usage in descriptors
```json
{
	"name": "myNegativeNumber",
	"type": "number[numbers.negative]"
}
```

----

### Even
Checks that a value divides by 2.  
Applies to: [number](builtins/types?id=number).  
Syntax: `even`.

##### Usage in descriptors
```json
{
	"name": "myEvenNumber",
	"type": "number[numbers.even]"
}
```

----

### Odd
Checks that a value isn't divides by 2.  
Applies to: [number](builtins/types?id=number).  
Syntax: `odd`.

##### Usage in descriptors
```json
{
	"name": "myOddNumber",
	"type": "number[numbers.odd]"
}
```

----

### Divided By
Checks that a value is divided by a specific divisor.  
Applies to: [number](builtins/types?id=number).  
Syntax: `dividedBy (divisor number)`.

##### Usage in descriptors
```json
{
	"name": "dividedByTen",
	"type": "number[numbers.dividedBy 10]"
}
```

----

## Strings
### Exact Length
Checks that the length of the value is exactly a specific length.  
Applies to: [string](builtins/types?id=string).  
Syntax: `exactLength (num number)`.

##### Usage in descriptors
```json
{
	"name": "10CharsString",
	"type": "string[strings.exactLength 10]"
}
```

----

### Minimum Length
Checks that the length of the value is not below a specific threshold.  
Applies to: [string](builtins/types?id=string).  
Syntax: `minLength (num number)`.

##### Usage in descriptors
```json
{
	"name": "10CharsOrMoreString",
	"type": "string[strings.minLength 10]"
}
```

----

### Maximum Length
Checks that the length of the value is up to a specific limit.  
Applies to: [string](builtins/types?id=string).  
Syntax: `maxLength (num number)`.

##### Usage in descriptors
```json
{
	"name": "10CharsOrLessString",
	"type": "string[strings.maxLength 10]"
}
```

----

### Between Length
Checks that the length of the value is between min and max.  
Applies to: [string](builtins/types?id=string).  
Syntax: `between (min number) (max number)`.

##### Usage in descriptors
```json
{
	"name": "10to20CharsString",
	"type": "string[strings.between 10 20]"
}
```

----

### Pattern/RegEx
Checks that the value has a specific pattern.  
Applies to: [string](builtins/types?id=string).  
Syntax: `regex ('pattern' string)` or `regex ('pattern' string) (flags string)`.  
Note: the pattern must be surrounded by single quotes.

##### Usage in descriptors
```json
{
	"name": "alphaString",
	"type": "string[strings.regex '^[a-bA-B]+$']"
}
```
```json
{
	"name": "alphaIgnoreCaseString",
	"type": "string[strings.regex '^[a-b]+$' i]"
}
```

----

## Collections
### Exact Size
Checks that the collection has a specific size.  
Applies to: [list](builtins/types?id=list), [map](builtins/types?id=map).  
Syntax: `exactSize (size number)`

##### Usage in descriptors
```json
{
	"name": "5ItemsList",
	"type": "list[collections.exactSize 5]"
}
```
```json
{
	"name": "5ItemsStringMap",
	"type": "map<string>[collections.exactSize 5]"
}
```

----

### Minimum Size
Checks that the collection is at least at a specific size.  
Applies to: [list](builtins/types?id=list), [map](builtins/types?id=map).  
Syntax: `minSize (size number)`

##### Usage in descriptors
```json
{
	"name": "minOf5ItemsList",
	"type": "list[collections.minSize 5]"
}
```

----

### Maximum Size
Checks that the collection is at most at a specific size.  
Applies to: [list](builtins/types?id=list), [map](builtins/types?id=map).  
Syntax: `maxSize (size number)`

##### Usage in descriptors
```json
{
	"name": "upTo5ItemsList",
	"type": "list[collections.maxSize 5]"
}
```