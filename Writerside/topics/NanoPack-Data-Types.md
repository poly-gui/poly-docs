# NanoPack Data Types

NanoPack supports numerous data types.

## Number types

NanoPack supports the following data types:

- 8-bit integers: `int8`
- 32-bit integers: `int32`
- Double-precision floating-points: (Doubles): `double`

## Strings

NanoPack also supports UTF8-encoded strings. Use `string` as the type keyword.

## Messages

NanoPack supports using other NanoPack messages as types.
A message field can store a message of another message type, or even of its own type (recursive types).

To use another message as a type, simply use its name as the type name. No import statement is required to make it available.

> If you use other messages as types, make sure their schema files are compiled together with the schema file they are used in
> 
{style="note"}

## Arrays

NanoPack supports arrays of any NanoPack types. The array can store up to 4,294,967,296 elements.

Simply add a pair of square brackets `[]` to the end of a type to make it an array of that type.
For example, a string array is declared as `string[]`.

NanoPack also supports nested arrays. `string[][]`, for example, declares an array of string arrays.

## Maps

Maps are currently being implemented!
