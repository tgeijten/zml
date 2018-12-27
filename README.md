# What is ZML?
ZML is a concise-yet-readable file format for storing data. This is what it looks like:

```
# This is a comment
example {
  name: "Spacy Name"
  age: 42
  hierarchy {
    fruits [ apple pear banana ]
    position { x: 10 y: 20 z: 30 }
  }
}
```

When needed, ZML can become space-efficient without changing any of parsing rules:
```
example{name:"Spacy Name" age:42 hierarchy{fruits[apple pear banana] position{x:10 y:20 z:30}}
```

ZML has language-level support for *including* other files at any place in the hierarchy:
```
example {
  some_data = 99
  <-- some_other_file.zml # values are inserted in place, in-between existing nodes
}
```

To import children of a specific node from another file, use:
```
<-- some_other_file.zml@some_node@3 // includes the third child of some_node
```

Previous values can be referenced to with the **@** character
```
CONSTANT: 42

example
{
  important_value = @CONSTANT
}
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

# Why ZML?
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

