# Defining Messages

Every NanoPack message is defined in a NanoPack schema, which is just a YAML config object describing the message.

## Schema Syntax

```yaml
<MessageNameInPascalCase>:
  typeid: 1 # required
  <my_field>: <data-type>:<field-number>
  <my_second_field>: <data-type>:<field-number>
  # ...
  <my_last_field>: <array-data-type>[]:<field-number>
```

> Angle bracket denotes a placeholder, and is not part of the syntax.
>
{style="note"}

### Type ID

The reserved `type_id` field specifies the <a href="Binary-format.md" anchor="type-id"></a> of the message.
It has to be an integer from 1 to 4,294,967,296. **Every message is required to have a type ID.**

### Field Number

Each field must have an assigned number, similar to Protocol Buffer.
The number of a field is specified by the `:<number>` syntax, after the type of the field.

Different fields cannot share the same field number.

> Although not required, it is recommended that the fields are ordered by their field number, in ascending order.
>
{style="tip"}

### Data types

NanoPack supports various primitive data types. Other messages can also be used as the message type, without any special import syntax,
but schema of the message type used must be compiled together with the schema it is used in.
For example, if `MessageA` in `MessageA.yaml` uses `MessageB` in `MessageB.yaml` as a type for one of `MessageA`'s field,
`MessageA.yaml` and `MessageB.yaml` must be compiled together in one go.

To denote an array, simply add `[]` after the data type. For example, `string[]` denotes a string array of unknown size.

[Data type reference](NanoPack-Data-Types.md)

## Example schema

Below is a simple NanoPack schema that defines a message called `Person`:

```yaml
Person:
  typeid: 1
  first_name: string:0
  last_name: string:1
  age: int8:2
  friends: Person[]:3
```

`Person` has the following properties:

{type="narrow"}
typeid
: Person has a type ID of 1

first_name
: A string field with a field number of 0

last_name
: A string field with a field number of 1

age
: A field that stores an 8-bit integer and has a field number of 2

friends
: A field that stores an array of `Person`s and has a field number of 3.
