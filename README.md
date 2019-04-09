# ZSON
ZSON is a **concise-yet-readable file format** for storing program data. It's similar to JSON or XML, but easier on the eyes and more concise. ZSON uses whitespace to separate items, and requires quotes only in special cases.

## Example
ZSON looks like this:
```
example {
  name = "Spacy Name"
  age = 42
  properties {
    fruits [ apple pear banana ]
    position { x = 10 y = 20 z = 30 }
  }
}
```

JSON, on the other hand, looks like this:
```
"example": {
  "name": "Spacy Name",
  "age": 42,
  "properties": {
    "fruits": [ "apple", "pear", "banana" ],
    "position": { "x": 10, "y": 20, "z": 30 }
  }
}
```

## Specification
* ZSON files are valid UTF-8 documents
* ZSON keys are case-sensitive
* Whitespace means tab ('\t', 0x09) or space (0x20)
* Newline means LF ('\n', 0x0A) or CR ('\r', 0x0D) or CRLF (0x0D0A)
* Values containing whitespace or special characters must be surrounded by double-quotes ("")
* Special characters can be written using the '\' character, only when surrounded by double-quotes

## Features
### Everything is a string
ZSON values are type agnostic strings, since your code knows best how to interpret your data. Special characters are supported through backslashes and quotes:
```
example {
  "Keys or values with whitespace" = "must have quotes"
  also = "These characters need to be placed in quotes: { } [ ] # #{ #} @ << >>"
  special = "Special \"characters\" require a \\"
}
```

### Comments
Comments can be both single line (`#`) and multi-line (`##`):
```
example {
  # Single-line comment
  name = "Spacy Name" # Trailing comment
  ## all_this_stuff {
    has_been [ 1 12 67 ]
    commented_out { x = 10 y = 20 z = 30 }
  }
}
```

### Including other files
ZSON has language-level support for *including* other files at any place in the hierarchy, allowing you to compose your data from multiple files:
```
example {
  # Include values in-place from another file
  << some_other_file.zson  >>
}
```

To import children of a specific node from another file, use:
```
# Includes the second child of example/properties
<<< example.zson@example/properties/@2 >>>
```

### Referencing items
Previously defined values or nodes can be referenced to with the **@** character
```
my_constant: 42
important_value = @my_constant
another_important_value = @my_constant
```

### Compact notation
When needed, ZSON can become pretty space-efficient without changing any of parsing rules (useful for serialization / deserialization):
```
example{name="Spacy Name" age=42 hierarchy{fruits[apple pear banana] position{x=10 y=20 z=30}}
```

## Frequently asked questions (FAQ)
### Why?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Yes, one would think so. But all alternatives have something not quite right (apart from not supporting includes):
* **XML** is really verbose and hard to edit
* **JSON** has lots of superfluous quotes and comma's
* **CSON** gets the idea, but still needs too much quotes and tabs
* **YAML** is pretty difficult to parse
* **TOML** uses `[parent.child]` for child nodes, not suitable for deep hierarchies

Here's a table with a completely objective comparison:

| Feature             | XML | JSON | CSON | YAML | TOML | **ZSON**|
| --------            | --- | ---- | ---- | ---- | ---- | ---- |
| Easy on the eyes    | --  | +    | ++   | +    | ++   | **++** |
| Concise             | -   | +/-  | +    | ++   | +    | **++** |
| Easy to edit        | -   | +    | ++   | ++   | +    | **++** |
| Simple to parse     | -   | +    | +/-  | --   | +    | **++** |
| Include other files | --  | --   | --   | --   | --   | **++** |

### Performance
*If everything is a string, doesn't that make everything really slow?*

No, it really doesn't. Casting from string to numbers or anything else is really fast on modern processors. Plus, this approach allows the implementation to simply keep all the data in a single block of memory, which makes it super cache-friendly.

### What does ZSON mean?
ZSON is an acronym for Zeeky String-based Object Notation. Now you know :-)

### What are the ZSON objectives?
* **Concise** -- nobody likes too much typing
* **Easy on the eyes** -- designed for reading and writing by humans
* **Easy to parse** -- a fully featured ZSON parser can be written in <100 lines of code
* **Agnostic about data types** -- your code knows how to interpret your data, so everything is a string
* **Language-level support for includes and references** -- distribute your configuration across multiple files

