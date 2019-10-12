# ZML
ZML is a **concise-yet-readable file format** for storing data. Its goal is to be similar to JSON or XML, but more concise and easier on the eyes. Key features include:
* No redundant commas or quotes, items are separated by **whitespace**
* Files can **include** other ZML files
* Support for global reusable **constants**

A fully featured parser of the ZML format is included in the [xo library](https://github.com/tgeijten/xo).

## Example
ZML:
```
age = 42
name = "Spacey Name"
properties {
  fruits [ apple pear banana ]
  position { x = 10 y = 20 z = 30 }
}
```

JSON:
```
{
  "age": 42,
  "name": "Spacey Name",
  "properties": {
    "fruits": [ "apple", "pear", "banana" ],
    "position": { "x": 10, "y": 20, "z": 30 }
  }
}
```

XML:
```
<root>
  <age>42</age>
  <name>Spacey Name</name>
  <properties>
    <fruits>apple pear banana</fruits>
    <position x = "10" y = "20" z = "30"/>
  </properties>
</root>
```

## Specification
* ZML files are valid UTF-8 documents
* ZML keys and values are case-sensitive
* Newline means LF (`\n`, 0x0A) or CR (`\r`, 0x0D) or CRLF (0x0D0A)
* Whitespace means tab (`\t`, 0x09), space (0x20) or Newline
* Values containing whitespace or special characters must be surrounded by double-quotes (`""`)
* Inside double quotes, special characters are preceded by a backslash: `"\n \r \t \" \\"`

## Features
### Everything is a string
ZML values are type-agnostic strings, since your code knows best how to interpret your data. Special characters are supported through backslashes and quotes:
```
example {
  "Keys or values with whitespace" = "must have quotes"
  also = "These characters need to be placed in quotes: { } [ ] # @ << >>"
  special = "Special \"characters\" require a \\"
}
```

### Including files
ZML has language-level support for *including* other files at any place in the hierarchy, allowing you to compose your data from multiple files:
```
example {
  # Include values in-place from another file
  << some_subdirectory/some_other_file.zson  >>
}
```

To import children of a specific node from another file, use:
```
<< example.zml@some_node >> # Includes "some_node" from example.zml
<< example.zml@parent_node/@3 >> # Includes the third child of "parent_node"
```

### Constants
You can set multiple keys to one specific value by using **constants**. These are declared within the current scope using the `$` character:
```
objects {
  $some_constant = 42
  o1 = $some_constant
  o2 = $some_constant
  o3 = $some_constant
}
```

### Comments
Comments can be both single line (`#`) and multi-line (`###`):
```
example {
  # Single-line comment
  name = "Spacy Name" # Trailing comment
  ### all_this_stuff {
    has_been [ 1 12 67 ]
    commented_out { x = 10 y = 20 z = 30 }
  } ###
}
```

### Identical keys are allowed
ZML allows keys in a group to be identical, so you can define groups of objects with multiple instances of the same type:
```
scene_objects {
  cube { size = 1 }
  sphere { size = 1 }
  sphere { size = 0.5 }
}
```

### Compact notation
ZML can get pretty space-efficient without changing any of parsing rules (useful for streaming):
```
age=42 name="Spacy Name" hierarchy{fruits[apple pear banana] position{x=10 y=20 z=30}}
```

## Frequently asked questions (FAQ)
### Why?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Yes, one would think so. But all alternatives have something not quite right (apart from not supporting includes and references):
* **XML** is really verbose and hard to edit
* **JSON** has lots of superfluous quotes and comma's and does not allow identical keys
* **CSON** gets the idea, but still needs too much quotes and tabs and does not allow identical keys
* **YAML** is pretty difficult to parse and isn't great for deep hierarchies
* **TOML** uses `[parent.child]` for child nodes, not suitable for deep hierarchies

Here's a table with a completely objective comparison:

| Feature             | XML | JSON | CSON | YAML | TOML | **ZML**|
| --------            | --- | ---- | ---- | ---- | ---- | ---- |
| Easy on the eyes    | --  | +    | ++   | +    | ++   | **++** |
| Concise             | -   | +/-  | +    | ++   | +    | **++** |
| Easy to edit        | -   | +    | ++   | ++   | +    | **++** |
| Simple to parse     | -   | +    | +/-  | --   | +    | **++** |
| Include other files | --  | --   | --   | --   | --   | **++** |

### Performance
*If everything is a string, doesn't that make everything really slow?*

No, it really doesn't. Casting from string to numbers or anything else is really fast on modern processors. Plus, this approach allows an implementation to easily keep all the data in contiguous memory, which makes it super cache-friendly.

### What does ZML mean?
ZML is an acronym for Zeeky Minimal Language. Now you know :-)

### What are the ZML objectives?
* **Concise** -- minimize redundant characters
* **Easy on the eyes** -- designed for reading and writing by humans
* **Easy to parse** -- a fully featured ZML parser can be written in <100 lines of code
* **Agnostic about data types** -- your code knows how to interpret your data, so everything is a string
* **Language-level support for includes and references** -- distribute your configuration across multiple files
