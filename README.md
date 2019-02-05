# ZSON
ZSON is a concise-yet-readable file format for storing data. I'ts like JSON, but without the cruft.

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

When needed, ZSON can become super space-efficient without changing any of parsing rules:
```
example{name="Spacy Name" age=42 hierarchy{fruits[apple pear banana] position{x=10 y=20 z=30}}
```

## Features
#### Include other files
ZSON has language-level support for *including* other files at any place in the hierarchy:
```
example {
  # Include values in-place from another file
  <<< some_other_file.zml  >>>
}
```

To import children of a specific node from another file, use:
```
# Includes the second child of example/properties
<<< example.zml@example/properties/@2 >>>
```

#### Reference items
Previously defined values can be referenced to with the **@** character
```
my_constant: 42

important_value = @my_constant
another_important_value = @my_constant
```

#### Comments
Comments can be both single line and multi-line (similar to comments in C/C++):
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

#### Special characters and whitespace
Finally, special characters are supported through backslashes and quotes, and multi-line text is also supported:
```
example {
  "Keys or values with whitespace" = "must have quotes"
  also = "These characters need to be placed in quotes: { } [ ] # #{ #} @ <<< >>>"
  special = "Special \"characters\" require a \\"
}
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
