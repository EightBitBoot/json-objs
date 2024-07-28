# Json Objects

A small module to add json serialization and deserialization to python classes.  
  
It additionally adds `__init__()`, `__repr__()`, and `__str__()` functions (a-la dataclass) if no user-defined implementations are found.  
  
**NOTE**: This currently doesn't handle nested json objects or tests if the class's members are json serialiazable.  Functionally, it calls `repr()` (indirectly) on each to get their json encoding.  
  
**NOTE**: (again like dataclasses) all members must have type annotations for them to be visable to json_object. Internally, it uses `obj.__annotations__` to iterate class members which only lists members that have type annocations.

### Added functions
| Name | Description |
|:----:|:------------|
|`__init__()`|If no user-defined __init__() is found within the class, a new one is added containing a parameter for each member variable (akin to dataclasses _without_ typechecking)|
|`__repr__()`|If no user-defined __repr__() is found within the class, a new one is added, returning a string in the format `<class-name>(<member-1>=<member-1-value>, ...)`|
|`__str__()`|If no user-defined __str__() is found within the class, a new one is added, returning a string resulting from the operation `json.dumps(self.to_json_dict(), indent=4)`|
|`from_json_dict(json_dict)`|(Class method) Return a new instance of the class, loading values from a dictionary (`{<member-name>: <member-value>, ...}`) akin to one `json.loads(...)` would return|
|`from_json_str(json_str)`|(Class method) Return a new instance of the class, loading values from a json string (akin to calling `<class-name>.from_json_dict(json.loads(json_str))`)|
|`to_json()`|Return a json representation of the object|
|`to_json_dict()`|Return a dictionary containing the values of all members (`{<member-name>: <member-value>, ...}`) (akin to calling `json.loads(<instance>.to_json())`)

### Example
```python
from json_objs import json_obj

@json_obj
class Test:
    foo: int
    bar: str

test = Test(3, "Hello world!")
json_str = test.to_json()
print(json_str)

test2 = Test.from_json_str(json_str)
print(repr(test2))
print(str(test2))
```

Output:
```
{"foo": 3, "bar": "Hello world!"}
Test(foo=3, bar=Hello world!)
{
    "foo": 3,
    "bar": "Hello world!"
}
```
