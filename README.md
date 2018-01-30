# ZML
ZML is a fileformat for storing structured hierarchical data. It's designed with the following goals in mind:
1. Easy to read, write and edit
2. Consise!
3. Easy to parse
4. Datatype agnostic

## Why?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Well, I've looked through many of them, and for all of them there's something I didn't like (I suppose I'm picky). Here are my gripes with popular existing languages:
### XML
* It's just too VERBOSE!

### json
* It's still not as concise as could be: the comma after each statement and the required quotes around each entry are annoying.
* It's not type agnostic
* The first two letters stand for 'javascript' (YUCK!)

### YAML
Another attempt of making 
* Pretty hard to parse
* I don't like the way it uses '-' for child nodes

### TOML
* Not a fan of how child nodes are handles

## here's wat zml looks like
Wihtout further ado, here's an example that shows everything there is to know about zml:
```
parent = {
  name = "A Name With Spaces"
  age = 12
  children = {
    array = [ 1 2 3 ]
    more-stuff = 4 ; this is a comment!
  }
  concise = { x = 10 y = 20 z = 30 }
}
```
  
