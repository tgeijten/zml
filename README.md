# What is zml?
zml is a human-readable file format for storing data. This is what it looks like:

```
example {
  name = "A Name With Spaces"
  age = 42
  hierarchy {
    lucky_numbers = [ 1 12 67 ]
    concise { x = 10 y = 20 z = 30 }
  }
  
  // this is a comment
  #include another_file.zml
  #merge yet_another_file.zml#some_node
}
```

If you want, zml can be made super-concise:
```
example{name="A Name With Spaces" age=42 hierarchy{lucky_numbers[1 12 67] concise{x=10 y=20 z=30}}
```


Some of zml's core qualities are:
1. It's easy on the human eye
2. Concise and clean
3. Type agnostic
4. Allows insertion and merging of other zml files

# Why ZML?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Yes, one would say so. But all alternatives have something not quite right:
* **XML** is verbose as hell and pretty impractical
* **JSON** is also not as concise as could be -- there's a lot of superfluous quotes and comma's
* **CSON** looks fine, but doesn't have any include options
* **YAML** just to parse and doesn't look that great
* **TOML** uses `[parent.child]` for child nodes, not suitable for deep hierarchies
