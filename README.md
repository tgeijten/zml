# What is ZML?
ZML is a human-readable file format for storing data. It's designed with the following goals in mind:
1. Easy on the human eye
2. Concise and practical
3. Type agnostic

# Why ZML?
*There's already so many languages like this! Why design ANOTHER one instead of using any of the existing languages?*

Well, I've looked through many of them, and for all of them there's something I didn't like (I suppose I'm picky). Here's my beef with some popular alternatives:

#### XML
* Verbose!
* Not easy on the eyes
* Hard to parse

#### JSON
* Not as concise as could be -- there's a lot of superfluous use of `,` and `"`
* The first two letters stand for 'javascript' (ugh)

#### CSON
* Not as concise as could be -- there's a lot of superfluous use of `,` and `"`
* The first two letters stand for 'javascript' (ugh)

#### YAML
Another attempt of making 
* Hard to parse
* I don't like the way it uses '-' for child nodes

#### TOML
* I don't like how it uses `[parent.child]` for child nodes, not suitable for deep hierarchies

# What does it look like?
Wihtout further ado, here's an example that shows everything there is to know about zml:
```
parent {
  name = "A Name With Spaces"
  age = 42 // this is a comment!
  children {
    an array = [ 1 2 3 ] // the '=' is optional!
  }
  super_concise { x = 10 y = 20 z = 30 } // multiple values on one row
  #include other_file.zml
}
```
