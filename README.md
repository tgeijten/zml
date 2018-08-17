# What is zml?
ZML is a human-readable file format for storing data. This is what it looks like:

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

If you want, ZML can be made super-concise without changing any of the grammar rules:
```
example{name="A Name With Spaces" age=42 hierarchy{lucky[1 12 67] concise{x=10 y=20 z=30}}
```

ZML has language-level support for including other files at any place in the hierarchy:
```
example {
  some_data = 99
  #include some_other_file.zml
}
```

ZML also allows merging of data from other files, great for defining defaults in another file:
```
example {
  #merge default_values.zml
  some_data = 99
}
```

Both #include and #merge can be used to import a specific node:
```
#include some_other_file.zml#some_node
#merge some_other_file.zml#some_node/some_child
}
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
  multi-line comment start */
}
```

Finally, special characters are supported through backslashes and quotes, and multi-line text is also supported:
```
example {
  name = "These characters need to be placed in quotes: {}[]#//"
  special = "Special \"characters\" require a \\"
  formatting = "Multi-line formatting is also supported. Just continue typing
                on the next line -- newline characters in quotes are transformed
                into a single spaces on parsing. Actual newline characters can be
                inserted using \n."
```

# Why ZML?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Yes, one would say so. But all alternatives have something not quite right:
* **XML** is verbose as hell and pretty impractical
* **JSON** is also not as concise as could be -- there's a lot of superfluous quotes and comma's
* **CSON** looks fine, but doesn't have any include options
* **YAML** just to parse and doesn't look that great
* **TOML** uses `[parent.child]` for child nodes, not suitable for deep hierarchies
