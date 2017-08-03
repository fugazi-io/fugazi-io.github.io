# Components
Components are the blocks with which fugazi can be extended (or plugged into).  
The client aims to be as "thin" as possible and only include what's necessary to run, with all other functionality 
added as components. That includes the basic components including types and commands.

## Types
Types are used to define what is the input that the command is expecting to receive and what is the output.  
The client has the following base types:
 - void
 - any
 - number
 - string
 - list
 - map

All additional types need to extend one of the base types using constraints.

## Constraints
Constraints are simple javascript functions which receive a value and checks whether or not it is valid 
for a certain type.  
By adding constraints to an existing type (doesn't have to be a base type) a new type is created.

For example, a new type can be created for an integer which is based on the number base type along with 
 a function which checks if the value is an integer.

## Converters
Converters are javascript functions which takes in a value of a certain type and returns a converted value 
of another type.

For example, convert int to float, list to string, etc.

## Commands
Commands are what fugazi is all about.  
They are divided to two:
 - Local commands: a javascript function that is loaded into the client and is executed in the terminal. 
 Local commands are usually good for utility functionality or data manipulation.
 - Remote commands: an http endpoint to which a request should be send. Remote commands are usually good for 
 fetching data from services/applications.

Each command has one or more syntax rules which define how the user needs to execute it. In addition, commands 
also declares the type of the result.

### Syntax Rules
A syntax rule is defined as a string consisting of keywords and parameters delimited by a space.  
Each parameter is defined using a name and a type.

When executing a command input by the user, the client will search for commands based on their syntax.

## Modules
A module is basically an aggregation of other components (including inner modules). You can look at it as a 
namespace which contains other components.  
In addition to the inner components, the module also contains other information:
 - Remote configuration: Where the http requests should be sent to
 - Module level variables: The ability to map between syntax params and runtime values

Modules can have the following flavours:
 - **LocalModule**: Loaded as a javascript file and it can only executes local commands
 - **RemoteModule**: Can be loaded as either a js or json, has a remote configuration and can have both local 
 and remote commands
 - **RootModule**: The main module which is loaded by the request of the user into fugazi. Can be either local 
 or remote and can include inner modules
 - **InnerModule**: Any module that has a parent