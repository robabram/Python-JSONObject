|badge1| |badge2| |badge3|

.. |badge1| image:: https://github.com/robabram/python-easy-json/actions/workflows/unittests.yaml/badge.svg
  :alt: Unittest Completion Status
.. |badge2| image:: https://raw.githubusercontent.com/robabram/python-easy-json/coverage-badge/coverage.svg?raw=true
  :alt: Code Coverage Status
.. |badge3| image:: https://img.shields.io/badge/python-v3.7%20|%20v3.8%20|%20v3.9%20|%20v3.10%20|%20v3.11-blue
  :alt: Python v3.7, v3.8, v3.9, v3.10, v3.11


****************************************************************************************
python-easy-json: simple, yet powerful, JSON/python dictionary to object deserialization  
****************************************************************************************

**python-easy-json** is a recursive JSON to python object deserializer with support for defining data models 
and casting data to python using type hint annotations. 

The python-easy-json JSONObject class can be used to:

- **Deserialize:** Recursively convert a JSON string or python dictionary to a python object
- **Serialize:** Export the object data to a JSON string or python dictionary
- **Type Hinting Integration:** Convert JSON values to specific types by creating models and type hints.
- **Define Data Models:** Create simple, yet powerful data models for working with data from any source
- **IDE Auto Completion:** IDEs with auto-completion and support python type hinting will auto-complete model properties


Get It Now
==========

    $ pip install python-easy-json

Why a another JSON to Object library?
=====================================
After years of python development, I grew tired of receiving data from APIs, database, csv files and so on and working 
with them as python dictionaries. The "simple" JSON deserializer library options I saw really didn't fit how easy I felt
that deserializing to a JSON object should be.  Additionally, I wanted to create simple data model classes and 
using python "Type Hinting" to define property value types. 


Simple Examples
===============

Just pass a JSON string or python dict argument to the JSONObject constructor.  In this example, we can switch from using dict key lookups to an array of JSONObjects.  

::

    for row in results:
        if row['the_key'][0]['another_key'] == 'the_value':
            ...

With JSONObject this may be re-written as below, using list comprehension. This makes the code more readable and less cluttered when working with complex dictionary structures in code.

::

    from python_easy_json import JSONObject

    for row in [JSONObject(r) for r in results]:
        if row.the_key[0].another_key == 'the_value':
            ...

Data from a JSON String

::

    from python_easy_json import JSONObject
    
    # JSON string
    obj = JSONObject('{"test_key": "test_value"}')
    print(obj.to_json())

    {"test_key": "test_value"}

Data from a python dictionary

::

    # Python dictionary
    obj = JSONObject({'test_key': 'test_value'})
    print(obj.to_json())

    {"test_key": "test_value"}


Data Models For Anything
========================

Using the python-easy-json JSONObject class, you can create data models, including deeply nested models and arrays, from any
JSON string or dictionary. Additionally, python "Type Hints" may be used to cast values to the type defined by the type 
hint annotations.

As a bonus; IDEs with auto-completion and support for python type hinting will auto-complete model properties as you type. 

This example shows how to define nested/child data models, including lists of nested data models.

::

    # Represents json from 'tests/test_data/nested_data_1.json'
    class CakeToppingTypeModel(JSONObject):
        id: int = None
        type: str = None    
    
    class CakeBatterTypeModel(JSONObject):
        id: int = None
        type: str = None    
    
    class CakeBatterModel(JSONObject):
        batter: List[CakeBatterTypeModel] = None    
    
    class CakeModel(JSONObject):
        id: str = None
        type: str = None
        name: str = None
        ppu: float = None
        batters: CakeBatterModel = None
        topping: List[CakeToppingTypeModel]

    cake = CakeModel(data)
    print(f'Cake: {cake.name} ({len(cake.batters.batter)} ingredents).') 

    Cake: Devil's Food Cake (4 ingredients).


Type Hints Automatically Convert JSON Values
============================================
If a model has been defined and the properties have python Type Hint annotations, the JSONObject can convert values 
to the annotation types.

::

    from datetime import datetime 

    class TimestampModel(JSONObject):
        id: int = None
        timestamp: datetime = None

    data = {'id': "123", "timestamp": "2022-09-19 10:11:01.123456"}
    obj = TimestampModel(data, cast_types=True)

    if obj.id > 0:
        print(f"ID: {obj.id}: {obj.timestamp.strftime('%b %d, %Y @ %H:%M:%S %p')}")

    $ ID: 123: Sep 19, 2022 @ 10:11:01 AM

Documentation
=============

**JSONObject Class**

::

    JSONObject.__init__(data: Union[Dict, str, None] = None, cast_types: bool = False, ordered: bool = False)
        Load the dictionary or JSON string data argument into ourselves as properties.
        :param data: Dictionary or valid JSON string.
        :param cast_types: If properties of this class are type annotated, try to cast them.
        :param ordered: Use OrderedDict() if set, otherwise use dict(). For python <= 3.6.

    JSONObject.to_json(indent: int = None)
        Export stored data as a json string.
        :param indent: Positive integer value for formatting JSON string indenting.
        :returns: JSON string

    JSONObject.to_dict(recursive: bool = True, dates_to_str: bool = False)        
        Export stored data as a python dictionary object.
        :param recursive: Boolean, recursively convert nested JSONObjects to a dict
        :param dates_to_str: Boolean, convert all date or datetime values to string.
        :returns: dictionary object

    
Project Links
=============

- PyPI: https://pypi.python.org/pypi/python-easy-json
- Issues: https://github.com/robabram/python-easy-json/issues

License
=======

MIT licensed. See the bundled `LICENSE <https://github.com/robabram/python-easy-json/blob/main/LICENSE>` file for more details.


Unittest Data
-------------

Testing JSON data for examples and unittests sourced from: https://opensource.adobe.com/Spry/samples/data_region/JSONDataSetSample.html