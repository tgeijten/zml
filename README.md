# ZSON
ZSON is a concise-yet-readable file format for storing data. It's like JSON, but without the cruft. ZSON uses whitespace to separate items, and requires quotes only in special cases.

## Objectives
* Concise
* Easy on the eyes
* Easy to parse
* Agnostic about data types
* Language-level support for includes and references

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

While the corresponding JSON looks like this:
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

## Features
### Including other files
ZSON has language-level support for *including* other files at any place in the hierarchy:
```
example {
  # Include values in-place from another file
  <<< some_other_file.zson  >>>
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

### Comments
Comments can be both single line and multi-line:
```
example {
  # Single-line comment
  name = "Spacy Name" # Trailing comment
  #{ Group comment
  this_stuff {
    has_been [ 1 12 67 ]
    commented_out { x = 10 y = 20 z = 30 }
  }
  #}
}
```

### Special characters and whitespace
Finally, special characters are supported through backslashes and quotes, and multi-line text is also supported:
```
example {
  "Keys or values with whitespace" = "must have quotes"
  also = "These characters need to be placed in quotes: { } [ ] # #{ #} @ <<< >>>"
  special = "Special \"characters\" require a \\"
}
```

### Compact notation for serialization / deserialization
When needed, ZSON can become pretty space-efficient without changing any of parsing rules:
```
example{name="Spacy Name" age=42 hierarchy{fruits[apple pear banana] position{x=10 y=20 z=30}}
```

## Why ZSON?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Yes, one would say so. But all alternatives have something not quite right -- apart from not supporting includes:
* **XML** no explanation needed, right?
* **JSON** lots of superfluous quotes and comma's
* **CSON** Gets the idea, but still needs to much quotes and tabs
* **YAML** is just very hard to parse
* **TOML** uses `[parent.child]` for child nodes, not suitable for deep hierarchies

| Feature             | XML | JSON | CSON | YAML | TOML | **ZSON**|
| --------            | --- | ---- | ---- | ---- | ---- | ---- |
| Easy on the eyes    | --  | +    | ++   | +    | ++   | **++** |
| Concise             | -   | +/-  | +    | ++   | +    | **++** |
| Easy to edit        | -   | +    | ++   | ++   | +    | **++** |
| Simple to parse     | -   | +    | +/-  | --   | +    | **++** |
| Include other files | --  | --   | --   | --   | --   | **++** |
