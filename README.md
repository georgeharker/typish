[![image](https://img.shields.io/pypi/pyversions/typish.svg)](https://pypi.org/project/typish/)
[![Downloads](https://pepy.tech/badge/typish)](https://pepy.tech/project/typish)
[![Pypi version](https://badge.fury.io/py/typish.svg)](https://badge.fury.io/py/typish)
[![codecov](https://codecov.io/gh/ramonhagenaars/typish/branch/master/graph/badge.svg)](https://codecov.io/gh/ramonhagenaars/typish)

# Typish

* Functions for thorough checks on types
* Instance checks considering generics
* Typesafe Duck-typing

## Installation

```
pip install typish
```

## Content

### Functions

| Function | Description
|---|---
| ``subclass_of(cls: type, *args: type) -> bool`` | Returns whether ``cls`` is a sub type of *all* types in ``args``
| ``instance_of(obj: object, *args: type) -> bool`` | Returns whether ``cls`` is an instance of *all* types in ``args``
| ``get_origin`` | Return the "origin" of a generic type. E.g. ``get_origin(List[str])`` gives ``list``.
| ``get_args`` | Return the arguments of a generic type. E.g. ``get_args(List[str])`` gives ``(str, )``.
| ``get_alias`` | Return the ``typing`` alias for a type. E.g ``get_alias(list)`` gives ``List``.
| ``get_type`` | Return the (generic) type of an instance. E.g. a list of ints will give ``List[int]``.
| ``common_ancestor`` | Return the closest common ancestor of the given instances.
| ``common_ancestor_of_types`` | Return the closest common ancestor of the given classes.

### Types

| Type | Description
|---|---|
| ``T`` | A generic Type var.
| ``KT`` | A Type var for keys in a dict.
| ``VT`` | A type var for values in a dict.
| ``Empty`` | The type of emptiness (= ``Parameter.empty``).
| ``Unknown`` | The type of something unknown.
| ``Module`` | The type of a module.
| ``NoneType`` | The type of ``None``.

### SubscriptableType
This metaclass allows a type to become subscriptable.

*Example:*
```python
class MyClass(metaclass=SubscriptableType):
    ...
```
Now you can do:
```python
MyClass2 = MyClass['some args']
print(MyClass2.__args__)
print(MyClass2.__origin__)
```
Output:
```
some args
<class '__main__.MyClass'>
```

### Something
Define an interface with ``typish.Something``.

*Example:*
```python
Duck = Something['walk': Callable[[], None], 
                 'quack': Callable[[], None]]
```

Anything that has the attributes defined in ``Something`` with the right type is 
considered an instance of that ``Something`` (classes, objects, even modules...).

The builtin ``isinstance`` is supported as well as ``typish.instance_of``.
