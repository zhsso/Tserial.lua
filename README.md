## Tserial

### About
Tserial is a library for turning tables into strings ("serialization") and vice-versa. This has several uses, including making a game save/load system (you write all your game state to a file) and multiplayer (you can pass tables across the network). Tserial turns tables into Lua script, for maximum ease-of-use.

### Download
Direct from [Dropbox](http://dl.dropbox.com/u/3713769/web/Love/TLTools/Tserial.lua)

### Contact
Taehl - SelfMadeSpirit@gmail.com

### Setup
* Put Tserial.lua in your game's folder
* At the top of main.lua, add the line require"Tserial"

### FAQ

Q) Can Tserial pack a table that contains other tables?

A) Yes. Any number of nested tables is fine (it's suitable even for things like tile maps). However, Tserial can NOT handle self-referential tables (such as t={t} or t={{1,2,3},{t}}).

Q) What data types can/can't be handled by Tserial?

A) It will properly serialize strings, numbers, booleans, and tables. A table may have any of these types as either keys or values. Userdata and functions can't be serialized by default, but can be supported with a little extra work.

Q) How can I serialize userdata?

A) The second parameter of Tserial.pack is used for these. If it's true, Tserial will simply skip userdata and functions. If it's a function, this function will be used to serialize the data. If it's a table, the table will be used for serial lookups.

### Functions
#### Tserial.pack
    Tserial.pack(t, drop, indent)
Serializes a table into a string. Returns a string of Lua script which recreates the table.
##### table t
The table to be serialized. May not be self-referential.
##### table drop
What to do upon encountering data that can't be serialized, like userdata and functions. If absent, Tserial will throw an error upon encountering them. If drop is a function, then it will be called with one parameter (the data) and expect a string in return (so you can supply your own serializing function). If drop is a table, it will be used to look up serials (with data as keys and serials as values).
##### boolean indent
If true, output will be in "human-readable mode" with newlines and proper indentation. Otherwise, the string will all be on one line (resulting in smaller sizes).

#### Tserial.unpack
    Tserial.unpack(s, safe)
Turns a serial back into a table. Returns a table recreated from the string (or nil and an error message if safe is true and a malformed string was given).
##### string s
The serial to be turned into a table.
##### string safe
If true, all extraneous parts of the string will be removed, leaving only a table (this prevents running anomalous code when unpacking untrusted strings). Will also cause malformed tables to quietly return nil and an error message, instead of throwing an error (so your program can't be crashed with a bad string).
