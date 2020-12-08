# IniParser
A fast and easy to use C# .ini files parser.

# Usage
To start using this parser, download the newest release and add it as an assembly reference to your project.

### Make sure to add the appropriate `using` statement to your code!
```cs
using Ini;
```

There are two classes that are used to represent data: `Category` and `Entry`.
A `Category` is an object that contains its name and a list of entries (`List<Entry>`).
An `Entry` object can be treated as a `KeyValuePair<string, string>`, where the `Key` property contains the entry key and `Value` contains the entry value.
Remember that an `Entry` object holds these values as strings, so if you want to get an integer or boolean, you must convert it manually.

The parser accepts an array of lines from the .ini file and outputs a `Dictionary` where every key is a `string` representing the category name:
```cs
string[] lines = File.ReadLines("path/to/file.ini").ToArray();
Dictionary<string, Ini.Category> result = Ini.Parser.Parse(lines);
```

If an error occurres while parsing a file, the parser will throw an `ArgumentException` which will contain information about what went wrong and what line the parser had trouble with.

# Examples

Let's say we have a file that looks like this:
```ini
[UserConfig]
Name=John
Surname=Doe

[NetworkingStuff]
IP=192.168.0.1
Port=8080
```

The `result` variable would contain a dictionary where the keys list is `["MyCategory", "NetworkingStuff"]`.
However, `.Values` of `result` would be a list of `Category` objects. That way it's easy to get a category object just by knowing its name.

Let's say we want to get the entries of the `[UserConfig]` category.
```cs
Ini.Category user = result["UserConfig"];
```
Great! Now we have a `Category` object containing all the entries from the `[UserConfig]` section.
The `Category` class implements a method named `GetEntryByKey` which accepts a key string as the argument and returns an `Entry` object.
As mentioned before, the `Entry` class is just a container for a key and a value, so if you wanted to get the `Name` property of `[UserConfig]`, you could do this:
```cs
Ini.Category user = result["UserConfig"];
string name = user.GetEntryByKey("Name").Value;
```

If you want to convert this `Category` object into a dictionary, you can use this little snippet:
```cs
var result = new Dictionary<string, string>();
Ini.Category user = result["UserConfig"];
foreach (Ini.Entry entry in user.Entries) result[entry.Key] = entry.Value;
```
