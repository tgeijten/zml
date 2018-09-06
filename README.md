# What is ZML?
ZML is a concise yet human-readable file format for storing data. This is what it looks like:

```
example {
  name = "A Name With Spaces"
  age = 42 // this is a comment
  hierarchy {
    lucky = [ 1 12 67 ]
    concise { x = 10 y = 20 z = 30 }
  }
}
```

When needed, ZML can be very space-efficient without changing any of parsing rules:
```
example{name="A Name With Spaces" age=42 hierarchy{lucky[1 12 67] concise{x=10 y=20 z=30}}
```

ZML has language-level support for *including or merging* other files at any place in the hierarchy:
```
example {
  #include some_other_file.zml // values are inserted
  #merge default_values.zml // data from other file is inserted, skipping all existing nodes
  #replace overwrite_values.zml // values are inserted, replacing all existing nodes
  some_data = 99
}
```

#include, #merge and #replace can be used to import children of a specific node from another file:
```
#include some_other_file.zml#some_node
#include some_other_file.zml#some_node/some_child
```

Comments can be both single line and multi-line (similar to comments in C/C++):
```
example { // single line comment
  name = "A Name With Spaces"
  /* multi-line comment start
  hierarchy {
    lucky = [ 1 12 67 ]
    concise { x = 10 y = 20 z = 30 }
  }
  multi-line comment end */
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
* **XML** is verbose as hell and pretty impractical
* **JSON** is also not as concise as could be -- there's a lot of superfluous quotes and comma's
* **CSON** looks fine, but doesn't have any include options
* **YAML** just to parse and doesn't look that great
* **TOML** uses `[parent.child]` for child nodes, not suitable for deep hierarchies

| Feature             | XML | JSON | YAML |
| --------            | --- | ---- | ---- |
| Easy on the eyes    |  -  | +    | ++   |
| Concise             |  -  | +    | ++   |
| Easy to edit        |  -  | +    | ++   |
| Include other files | -   | +    | ++   |
| Simple to parse     |  -  | +    | --   |

