^title Maps
^category types

A map is an *associative* collection. It holds a set of entries, each of which
maps a *key* to a *value*. The same data structure has a variety of names in
other languages: hash table, dictionary, association, table, etc.

You can create a map by placing a series of comma-separated entries inside
curly braces. Each entry is a key and a value separated by a colon:

    :::dart
    {
      "George": "Harrison",
      "John": "Lennon",
      "Paul": "McCartney",
      "Ringo": "Starr"
    }

This creates a map that maps the first names of the Beatles to their last
names. Syntactically, in a map literal, keys can be any literal, a variable name, or a parenthesized expression. Values can be any expression. Here, we're using string literals for both keys and values.

*Semantically*, values can be any object, and multiple keys may map to the
same value. Keys have a few limitations. They must be one of the immutable
built-in [value types](values.html) in Wren. That means a number, string,
range, bool, or `null`. You can also use a [class object](classes.html) as a
key.

The reason for this limitation&mdash;and the reason maps are called "*hash* tables" in other languages&mdash;is that each key is used to generate a numeric *hash code*. This lets a map locate the value associated with a key in constant time, even in very large maps. Since Wren only knows how to hash certain built-in types, only those can be used as keys.

## Adding entries

You add new key-value pairs to the map by using the [subscript operator][]:

    :::dart
    var capitals = {}
    capitals["Georgia"] = "Atlanta"
    capitals["Idaho"] = "Boise"
    capitals["Maine"] = "Augusta"

If the key isn't already present, this adds it and associates it with the given value. If the key is already there, this just replaces its value.

[subscript operator]: expressions.html#subscript-operators

## Looking up values

To find the value associated with some key, again you use your friend the subscript operator:

    :::dart
    IO.print(capitals["Idaho"]) // "Boise".

If the key is present, this returns its value. Otherwise, it returns `null`. Of course, `null` itself can also be used as a value, so seeing `null` here doesn't necessarily mean the key wasn't found.

To tell definitively if a key exists, you can call `containsKey()`:

    :::dart
    var belief = {"nihilism": null}

    IO.print(belief["nihilism"]) // "null" though key exists.
    IO.print(belief["solipsism"]) // Also "null".
    IO.print(belief.containsKey("nihilism")) // "true".
    IO.print(belief.containsKey("solipsism")) // "false".

You can see how many entries a map contains using `count`:

    :::dart
    IO.print(capitals.count) // "3".

## Removing entries

To remove an entry from a map, call `remove()` and pass in the key for the entry you want to delete:

    :::dart
    capitals.remove("Maine")
    IO.print(capitals.containsKey("Maine")) // "false".

If the key was found, this returns the value that was associated with it:

    :::dart
    IO.print(capitals.remove("Georgia")) // "Atlanta".

If the key wasn't in the map to begin with, `remove()` just returns `null`.

If you want to remove *everything* from the map, just like with [lists][], you
can just call `clear`:

    :::dart
    capitals.clear
    IO.print(capitals.count) // "0".

[lists]: lists.html

## Iterating over the contents

The subscript operator works well for finding values when you know the key you're looking for, but sometimes you want to see everything that's in the map. For that, map exposes two methods: `keys` and `values`.

The first returns a [Sequence][] that [iterates][] over all of the keys in the map, and the second returns one that iterates over the values.

[sequence]: core/sequence.html
[iterates]: control-flow.html#the-iterator-protocol

If you want to see all of the key-value pairs in a map, the easiest way is to iterate over the keys and use each to look up its value:

    :::dart
    var stateBirds = {
      "Arizona": "Cactus wren",
      "Hawaii": "Nēnē",
      "Ohio": "Northern Cardinal"
    }

    for (state in stateBirds.keys) {
      IO.print("The state bird of ", state, " is ", stateBirds[state])
    }

This program will print the three states and their birds. However, the *order* that they are printed isn't defined. Wren makes no promises about what order keys and values will be iterated in when you use these methods. All it promises is that every entry will appear exactly once.
