# Binary Format

> Using NanoPack doesn't require learning about the underlying structure of a serialized NanoPack message.
> However, it might be helpful for debugging NanoPack related errors.
> 
{style="note"}

NanoPack's binary format is straightforward. Below is a diagram that gives an overview of how NanoPack messages are serialized:

![A diagram that shows the structure of a serialized NanoPack message.](nanopack_diagram.png)

Continue reading to learn more about each element in the binary.

## Endian-ness

NanoPack uses little-endian format. There is no particular reason why it is used.

## Type ID

The first 4 bytes of every NanoPack binary stores the **type ID** of a message.
This ID is simply a number specified in the message schema that is used to identify the type of the message.
Without the type ID, it is impossible to determine how to correctly decode the binary into the correct message type,
since many different types of messages can be exchanged on the wire.

Type IDs of messages have to be unique within the system in which the message will be exchanged.
In Poly's case, all the messages exchanged through the native channel have unique type IDs.
This is to ensure that the raw binary can be interpreted and de-serialized correctly.
That means, future messages that will be exchanged through the channel cannot reuse type IDs of existing messages.
The Type ID uniqueness only has to be maintained within that instance of native channel, so type ID collisions are fine,
so long as the offending messages are exchanged through a theoretical separate instance of the native channel, or through an entirely differently medium in a different context.

## Size Header

After the type ID follows the size header, which stores the data size of each field.
4 bytes are used to encode the data size of each field, so the total size of the size header is `4n` bytes,
where `n` is the number of fields the message contains.

The order of  the size information is determined by the field number
assigned to each field. A field that has a field number of 0 always comes first in the header, followed by field number 1, so on and so forth.

Fields that contain fixed-size numbers have static sizes.
For example, a field that stores a 32-bit integer always use 4 bytes, so its size header will always be the number 4:

```
0x4 0x0 0x0 0x0
```

On the other hand, fields that store, for example, strings, will have dynamic size, and their size headers will contain
the number of bytes used by the string.

### Partial Deserialization

Size header allows NanoPack to *partially de-serialize* a message.
That is, NanoPack is able to de-serialize one particular field, without having to de-serialize all the previous fields.
For example, to read the content of field 3, the offset can be calculated by adding the size of the type ID (4 bytes), size header (12 bytes), and the sizes of previous fields,
which can be obtained by reading their size headers:

```
offset for field 3 = 4 + (4 * 3) + <size of field 0> + <size of field 1> + <size of field 2>
```

## Data Section

The data section follows immediately after the size header. This is where the actual raw data of all the fields go.

## NanoPack Limits

- Since type ID is encoded with 4 bytes, there can be 4,294,967,296 unique message types within a system.
- Size information for each field is encoded with 4 bytes as well, so each field can store up to 4,294,967,296 bytes, which is roughly 4.295 gigabytes.
- Each message can contain unlimited fields.
