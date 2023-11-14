# NanoPack Data Types

NanoPack supports numerous data types.

## Number types

NanoPack supports the following data types:

- 8-bit integers: `int8`
- 32-bit integers: `int32`
- Double-precision floating-points: (Doubles): `double`

## Strings

NanoPack also supports UTF8-encoded strings. Use `string` as the type keyword.

## Booleans

Use `bool` to denote a boolean field.

## Arrays

NanoPack supports arrays of any NanoPack types. The array can store up to 4,294,967,296 elements.

Simply add a pair of square brackets `[]` to the end of a type to make it an array of that type.
For example, a string array is declared as `string[]`.

NanoPack also supports nested arrays. `string[][]`, for example, declares an array of string arrays.

## Maps

NanoPack supports maps as well. Only strings and number types can be used as map keys, but map values can be of any type.
Maps can store up to 4,294,967,296 entries.

To declare a map type, use the following syntax:

```
<key-type:value-type>
```

For example, to declare a field `my_field` that stores map of string to 32-bit integers:

```yaml
my_field: <string:int32>:1
```

## Optional

A type can be made optional by adding a question mark (`?`) at the end of a type:

```yaml
# an optional array of string
field_1: string[]?:1

# an array of optional strings
field_2: string?[]:2
```

Except keys of maps, any type can be made optional.

## Messages

> This is still currently being implemented!
>
{style="warning"}

NanoPack supports using other NanoPack messages as types.
A message field can store a message of another message type, or even of its own type (recursive types).

To use another message as a type, simply use its name as the type name. No import statement is required to make it available.

> If you use other messages as types, make sure their schema files are compiled together with the schema file they are used in
>
{style="note"}

> Fields that have recursive types (i.e. fields that use the message it is in as its type) must be optional.
>
{style="note"}

## Type nesting

Since arrays and maps can store values of any type, you can go ham with the type!

```yaml
CrazyMessage:
  # Array of map of string to int32
  field_1: <string:int32>[]:0

  # Map of int32 to string arrays
  field_2: <int32:string[]>:1

  # Array of map of strings to string arrays
  field_3: <string:string[]>[]:2

  # Map of strings to map of int32 to int8
  field_4: <string:<int32:int8>>:4
```

> Please use this feature responsibly.
> 
{style="note"}
