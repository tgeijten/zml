# zml
Zasty/Zappy/Zany Minimal Language.

zml is a fileformat for storing structured hierarchical data. It's designed with the following goals in mind:
1. Easy to read
2. Easy to write and edit
3. Easy to parse

## why?
*But... there's already so many languages like this! Why design yet ANOTHER new one?*

Well, here's an overview of what I don't like about existing languages:
### xml

### json

### YAML

### TOML



## here's wat zml looks like
Wihtout further ado, here's an example that shows everything there is to know about zml:
```
parent {
  name = "Billy the Kid"
  age = 12
  objects {
    digits = "1 2 3"
    more-stuff = 4
  }
  concise { x = 10 y = 20 z = 30 }
}
```
  
