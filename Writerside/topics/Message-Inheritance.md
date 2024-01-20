# Message Inheritance

A message can inherit from another message, called the *parent message*.
By inheriting a message, all the fields of the parent message will be available in the message (child message) that inherits the parent message.

## Syntax

To inherit from a message, simply add `::<ParentMessageName>` after the name of the child message.

## Example

Let's create a simple message called `GameObject` that represents an object in a hypothetical game:

```yaml
GameObject:
  typeid: 1
  id: number:0
  position: number[]:1
```

A special type of `GameObject` can be created by inheriting `GameObject`:

```yaml
Block::GameObject:
  typeid: 2
  durability: number:0
  material: string:1
```

Now, all the fields defined in `GameObject` are accessible in `Block` as well.

## Polymorphism

One of the primary benefits of inheritance is being able to group related types and store it in a field or a container.
Consider the following example:

```yaml
Box::GameObject:
  typeid: 3
  content: GameObject[]:0
```

The `content` field stores all `GameObject`s into an array, which can be `GameObject` itself
and also any message that inherits `GameObject`, such as `Block` or `Box`.

### Generated Code

The generated code will correctly describe the inheritance describe in the schemas. For example, in Swift,
`GameObject` will be a class that inherits `NanoPackMessage`:

```Swift
class GameObject: NanoPackMessage {
  // ...
}
```

and both `Block` and `Box` will be classes that extend `GameObject`:

```Swift
class Block: GameObject {
  // ...
}

class Box: GameObject {
  // ...
  let content: [GameObject]
  // ...
}
```

How the inheritance behavior is generated for each language is described in details in their corresponding guides. 
