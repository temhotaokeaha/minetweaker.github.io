# ZenCode specification
ZenCode is a multi-purpose scripting and programming language intended for a far wider use than just MineTweaker. In future, the scripting system will be hosted on a separate website, but for now, its complete specification will be available here.

Contents
- [Introduction](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=Statements-,Introduction,-The%20purpose%20of)
- [Scripts](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=language%E2%80%99s%20casting%20rules.-,Scripts,-ZenCode%20projects%20are)
- [Types](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=File%3AGrammar%20scriptelement.png-,Types,-ZenCode%20knows%20the)
- [Primitive types](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=generic%20type%20parameters.-,Primitive%20types,-The%20following%20primitive)
- [Arrays](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=return%20a%20value-,Arrays,-Arrays%20(lists)%20contain)
- [Associative Arrays](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=1%2C%202%5D.fixed%3B-,Associative%20Arrays,-Being%20a%20very)
- [The any type](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=with%20the%20%7B%7D%20notation.-,The%20any%20type,-A%20special%20type)
- [Type handling](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=it%20is%20converted.-,Type%20handling,-Specifying%20the%20type)
- [Statements](https://minetweaker.github.io/wiki/ZenCode_specification#:~:text=static%20type%20system.-,Statements,-The%20traditional%20statements)

# Introduction
The purpose of ZenCode is to make it possible for non-programmers (or programmers with little experience) to use a simple scripting language while allowing more experienced programmers to take advantage of all the speed and features offered by modern object-oriented languages. Currently, ZenCode compiles at runtime to java bytecode, but in future, other target platforms should be supported.

The idea of ZenCode is that it can be used by a wide range of people. While experienced programmers can easily understand every detail that goes on under the hood, less experienced programmers and non-programmers could simply follow the tutorials and magically "have everything work" without having to bother about the details. For instance, ZenCode can be written with or without specifying data types. When used without types, the scripting engine can infer the types in most cases and use runtime-typed values in cases where it couldn't. It wouldn't be optimal in terms of speed, but it would work without having to teach the non-programmer all the details about everything in the language.

Meanwhile, more advanced programmers can specify types for those places where it can't be inferred, know everything that goes on under the hood, and write code that's both fast and clean. They would be able to write their advanced logic and make it available in the form of easily used APIs for less experienced users. This is particularly interesting in the modding world, where there is a mix of users with different skill levels. Making a Minecraft mod requires knowledge of both Java and Forge, which can be hard to learn for a non-programmer. However, many people would like to make simple modifications to the game - modifications that are difficult to provide with a graphical user interface. A scripting language would be needed, but shouldn't need a user to know what classes and interfaces, be aware of the difference between a float and double or about the language's casting rules.

# Scripts
ZenCode projects are organized as a set of scripts (ending with the .zs extension). Each script may contain:

- Code statements, such as function and method calls, variable assignments and control flow statements - Function definitions - Class, interface, struct and enum definitions - Include statements for other scripts

Additionally, at the beginning of a script, a package declaration can be provided. Adding a package declaration makes any public and exported functions available to other scripts. If there is no package declaration, every declaration will only be visible to the script itself.

Example script 1: (prints the text "Hello world")
```
var text = "Hello world";
print(text);
```
Example script 2: (makes a function add publicly available in the package myserver.util)
```
package myserver.util;

export function add(x, y)
{
  return x + y;
}
```
Example script 3: (includes a second script)
```
package myserver.util;

include "otherscript.zsi";
```

### PNG files that are not archived on Archive.org...
File:Grammar script.png

File:Grammar importedscript.png

File:Grammar scriptelement.png


# Types
ZenCode knows the following types:
- All signed types as available in java: byte, short, int, long
- Unsigned variants of these type: ubyte, ushort, uint, ulong
- Two floating-point types: float and double
- The string type: string
- Arrays; <type>[]
- Associative arrays (dictionaries, maps): <valuetype>[<keytype>]
- Classes: can contain fields, methods and operators, can extend a superclass, and implement any number of interfaces
- Interfaces: can contain interface methods and static methods, implement other interfaces and provide default implementations for some of its methods
- Structs: value types grouping a number of fields - can contain methods but can't extend classes or implement interfaces
- The special type void, indicating "no value"
Classes and interfaces can have generic type parameters.

# Primitive types
The following primitive types are available:
- byte: signed integer value, can contain values from -128 to 127
- short: signed integer value, can contain values from -32768 to 32767
- int: signed integer value, can contain values from –2147483648 to 2147483647
- long: signed integer value, can contain values from -9223372036854775808 to 9223372036854775807
- ubyte: unsigned integer value, can contain values from 0 to 255
- ushort: unsigned integer value, can contain values from 0 to 65535
- uint: unsigned integer value, can contain values from 0 to 4294967295
- ulong: unsigned integer value, can contain values from 0 to 18446744073709551615
- float: contains a single-precision floating-point value
- double: contains a double-precision floating-point value
- char: contains a single unicode character
- string: contains a string value (supports any unicode character)
- void: means the same as "no value", cannot contain any value but used to indicate that a method doesn't return a value

# Arrays
Arrays (lists) contain a series of values. Arrays can be constructed with the array expression (e.g. [value1, value2]) and can be nested.

A single-dimensional array type can be defined by adding [] to its base type:

 - ubyte[] defines an array of unsigned bytes (essentially a bytestream) - int[] defines an array of signed ints

Two kinds of multidimensional arrays can be defined:

 - int[][] defines a two-dimensional array of integers. Each element in the outer array can be of different length, for example [[1, 2], [3, 4, 5]] is a correct value. Each element of the outer array is a reference to a smaller array. - int[,] defines a rectangular array. For example, [[1, 2, 3], [4, 5, 6]] would be a valid rectangular array but [[1, 2], [3, 4, 5]] isn't. Internally, there is only one array.

Arrays can be modified after creation:
```
var array = [];
array += ["Hello"];
```
To make an array unmodifyable, use the .fixed accessor:
```
var array = [1, 2].fixed;
```

# Associative Arrays
Being a very common structure, the ability to define dictionaries mapping values of one type to values of another type is a first-class language feature in ZenCode. They are called associative arrays and can be created with the {} notation.

# The any type
A special type exists called "any". This is a dynamically typed value. Any value can be cast to the any type. When a value has been cast to any, all fields and methods will be kept - the value will keep the exact same behavior as the original value.

Variables of the any type are not statically checked: accessing methods or fields that do not actually exist will result in a runtime exception.

The any values can also be cast to its original type, as well as any of its superclasses or implemented interfaces, without needing an explicit cast. Trying to pass an invalid value will throw an exception as soon as it is converted.

# Type handling
Specifying the type of a variable, field or function parameter is always optional. The type will be inferred from its initialization value whenever possible and will default to the any type when unspecified.

Types and generic type arguments will be inferred from their context whenever possible. In a return statement, the type will be inferred from the method in which it resides and in a function call the method parameters are inferred from tje function being called (if there are no overloaded methods).

For arrays and associative arrays for which no type can be inferred, any[] and any[any] will be used by default. This means that creating an array of a specific type requires the following construct:
```
var myArray = ["Hello", "world"] as string[];
```
But if the type can be inferred, this is not needed:
```
doStrings(["Hello", "world"]);
```
Arrays can be passed to functions if the array base type is different. When such conversion is made, the converted array will always reflect the values in the original array: if the original is modified, the converted array will return the new values. If both base types can be converted to each other, the resulting array will be read-write. If not, the array will be read-only. Conversion between the array values is performed only when needed. This means that conversion errors will come up only when the values are actually being accessed:
```
function printInts(values as int[])
{
  for element in values {
    println(element);
  }
}
```
```
var myArray = [1, "Hello"];
printInts(myArray);
```
The code above will print 1 and then crash, complaining that "Hello" is not an int value.

Likewise, consider the following example:
```
functions addValues(values as any[])
{
  values += [1];
  values += ["Hi"];
}

var myArray = [] as int[];
addValues(myArray);
```
This snippet will successfully add 1 to the array but crash when "Hi" is added to it, complaining that "Hi" cannot be converted to an int.

This kind of behavior makes it possible to write a script quickly without ever having to bother about types whereas more complex applications, APIs and performance-critical code can use a static type system.

# Statements
The traditional statements can be used in scripts and function bodies:
```
var a = 1; // declares a variable a and intitalizes it to 1. The variable type is int.
val b = 2; // same as above, except b cannot be modified after initialization
a = 10;
if a > 2 { // conditional execution. No brackets needed.
  print("a is greater that 2");
}
```