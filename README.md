# table++

By Josh Klint

https://www.ultraengine.com

## About

This code is meant to bridge the gaps between dynamic data in C++, Lua, and JSON with an STL-like implementation of Lua tables.

Tables can be accessed by Lua using the [sol](https://github.com/ThePhD/sol2) library, but the tables themselves do not use Lua.

This is useful for dynamic application settings and user-defined settings added by script.

Only data types that can be written to JSON are supported.

The Lua ipairs() function currently does not work on C++ tables.

Currently there is a bug or design flaw that can be demonstrated as follows:

```lua
--Create a table
local a = Table()

--Add a child table
a["window"] = Table()

--Set a value on the child table
a["window"]["position"] = 300

--Print the value
Print(tostring(a["window"]["position"]))

--Grab the child table
local t = a["window"]

--Parent table goes out of scope lol
a = nil
collectgarbage()

-- try to access the child :_(
t["position"] = 5
```

This would be easy to solve with shared pointers, but it would destroy the nice neat C++ syntax we have now...

## License

This code may be used freely for any purpose.

If you use this code you must include this unmodified README file somewhere in your application directory or subdirectory where it is accessible to the end user.

This unmodified readme file must be included in any source code distribution.

You may not misrepresent the origin of this code.

## Examples

Here are some of the cool things you can do in C++ and Lua.

### Key / value in C++

```c++
table t;
t["health"] = 100;
t["windowsettings"] = {};
t["windowsettings"]["position"] = 3;

for (auto a : t)
{
    std::string s = a.first;
    Print(s);
    s = a.second;
    Print(s);
}
```

###  Arrays in C++

```c++
table arr;
arr.push_back(1);
arr.push_back(2);
arr.push_back(3);
arr.push_back(4);
arr.push_back(5);

for (int n = 0; n < arr.size(); ++n)
{
    Print(std::string(arr[n]));
}
```
### Mixing Key / value and array-style keys

```c++
table t;

t["health"] = 100;
t["windowsettings"] = {};
t["windowsettings"]["position"] = 3;

t.resize(10);
for (int n = 0; n < t.size(); ++n)
{
    t[n] = n;
}

//Insert an out-of-order index and it will get trimmed off in the resize call
t[12] = {};

t.resize(5);

Print("Size: " + String(t.size()));

for (auto a : t)
{
    std::string s = a.first;
    Print(s);
    s = a.second;
    Print(s);
}
```

###  Key / value in Lua

```lua
local a = Table()
a["health"] = 100
a["color"] = "blue"

for k,v in pairs(a) do
	Print(k ..": ".. tostring(v))
end
```

### Arrays in Lua

```lua
b = Table()
b[1] = 1
b[2] = 2
b[3] = 3
b[4] = 4

for n = 1, #b do
	Print(b[n])
end
```
