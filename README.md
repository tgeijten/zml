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
<parent>
  stuff = 1000
  name = "My Name Has Spaces"
  <inline_child name=louis name=dois name="space man"/>
  /* A comment with a closing tag */
  // A Single line comment
</> // This is a closing tag for parent. It's optional to write as </parent>.
```
  
