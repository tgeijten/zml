# ZML
ZML is a file format for storing structured data. It's designed with the following goals in mind:
1. Easy to read, write and edit
2. Consise
3. Easy to parse
4. Type agnostic

## Why ZML?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Well, I've looked through many of them, and for all of them there's something I didn't like (I suppose I'm picky). Here's my beef with some popular alternatives:

#### XML
* Verbose!
* Hard to parse

#### JSON
* Not as concise as could be, superfluous use of `,` and `"`
* It's not type agnostic
* The first two letters stand for 'javascript' (ugh)

#### YAML
Another attempt of making 
* Hard to parse
* I don't like the way it uses '-' for child nodes

#### TOML
* I don't like how it uses `[parent.child]` for child nodes, not suitable for deep hierarchies

## Example
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
