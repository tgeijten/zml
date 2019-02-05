# What is zson?
zson is a concise-yet-readable file format for storing data. This is what it looks like:

```
example {
  name: "Spacy Name"
  age: 42
  properties {
    fruits [ apple pear banana ]
    position { x: 10 y: 20 z: 30 }
  }
}
```

zson has language-level support for *including* other files at any place in the hierarchy:
```
# This is a comment
example {
  # Include values from another file
  <<< some_other_file.zml  >>>
}
```

To import children of a specific node from another file, use:
```
# Includes the second child of example/properties
<<< example.zml@example/properties/@2 >>>
```

Previously defined values can be referenced to with the **@** character
```
my_constant: 42

important_value = @my_constant
another_important_value = @my_constant
```

When needed, zson can become space-efficient without changing any of parsing rules:
```
example{name:"Spacy Name" age:42 hierarchy{fruits[apple pear banana] position{x:10 y:20 z:30}}
```

Comments can be both single line and multi-line (similar to comments in C/C++):
```
example { // single line comment
  name = "A Name With Spaces"
  /* comment-out the following bit
  hierarchy {
    lucky = [ 1 12 67 ]
    concise { x = 10 y = 20 z = 30 }
  }
  */
}
```

Finally, special characters are supported through backslashes and quotes, and multi-line text is also supported:
```
example {
  name = "These characters need to be placed in quotes: {}[]#//"
  special = "Special \"characters\" require a \\"
  formatting = "Multi-line formatting is also supported - just continue typing
                on the next line. When ZML is parsed, newline characters in quotes
                are replaced with single spaces on parsing (along with any leading
                or trailing white-space. Actual newline characters can be
                inserted using \n."
}
```

# Why zson?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Yes, one would say so. But all alternatives have something not quite right:
* **XML** is verbose as pandemonium and hard to edit
* **JSON** is also not as concise as could be -- there's a lot of superfluous quotes and comma's
* **CSON** Gets the idea, but still requires more quotes than needed and uses tab characters for grouping
* **YAML** is a nightmare to parse and doesn't look that great
* **TOML** uses `[parent.child]` for child nodes, not suitable for deep hierarchies

| Feature             | XML | JSON | CSON | YAML | TOML | **ZML**|
| --------            | --- | ---- | ---- | ---- | ---- | ---- |
| Easy on the eyes    | --  | +    | ++   | +    | ++   | **++** |
| Concise             | -   | +/-  | +    | ++   | +    | **++** |
| Easy to edit        | -   | +    | ++   | ++   | +    | **++** |
| Include other files | --  | --   | --   | --   | --   | **++** |
| Simple to parse     | -   | +    | +/-  | --   | +    | **++** |

