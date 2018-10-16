# Python Notes
### Classes
Creating classes.
```python
# Python 2 best practice
class Car(object):
    pass

# Python 3 equivalent
class Car:
    pass
```

### Organizing Larger Programs
#### Recommended Layout
```
project_name
|__ __main__.py
|__ project_name
|   |__ __init__.py
|   |__ more_source.py
|   |__ subpackage1
|       |__ __init__.py
|______ test
|        |__ __init__.py
|        |__ test_code.py
|__ setup.py
```

#### Singleton Pattern in Python
Modules are <b>singletons.</b>

```python
# registry.py

_registry = []

def register(name):
    _registry.append(name)

def registered_names():
    return iter(_registry)

# use_registry.py
import registry

registry.register('my name')

for name in registry.registered_names():
    print(name)
```

### Beyond Basic Functions

#### Callable instances

`__call__` can be used to define classes that, when instantiated, can be called using standard function calls. This may be useful when we want to create functions that may change state between calls.

```python
import socket

class Resolver:

    def __init__(self):
        self._cache = {}

    def __call__(self, host):
        if host not in self._cache:
            self._cache[host] = socket.gethostbyname(host)
        return self._cache[host]
```

```
>>> resolve = Resolver()        # Instantiate new method
>>> resolve('some-website.com') # Will build a cache
>>> resolve._cache              # will return the _cache dictionary
```

#### Callable classes
``` python
>>> resolve = Resolver()
#                      ^--- calling
#                 ^-------- a class

def sequence_class(immutable):
    if immutable:
        cls = tuple
    else:
        cls = list
    return cls

>>> seq = sequence_class(immutable = True) 
>>> t = seq("Timbuktu")
['T', 'i', 'm', 'b', 'u', 'k', 't', 'u']
```

#### Conditional Expressions
```python
# The code above can be simplified to:
def sequence_class(immutable):
    return tuple if immutable else list
```

#### Lambda Expressions
Anonymous functions. Be careful not to obfuscate code. Python 3 has eliminated parentheses in arguments. Zero or more arguments supported. `lambda:` denotes zero arguments.
* Lambdas are awkward or impossible to test.

```python
scientists = ['first_name, last_name' ... ]

sorted(scientists, key=lambda name: name.split()[-1])
```
#### Detecting callable objects
Use builtin function `callable()`.

#### Closures
Think of closures as the local scope "closing over" the objects it needs, preventing them from being garbage collected. Refer to `__closure__` method.

It helps to think of local functions as lambdas that are much more general and powerful.

```python
def enclosing():
    x = 'closed over'
    def local_func():
        print(x)
    return local_func

>>> lf = enclosing()
>>> lf()
closed over
>>> lf.__closure__
(<cell at ...: str object at ... >)
```

#### Function Factories
```python
def raise_to(exp):
    def raise_to_exp(x):
        return pow(x, exp)
    return raise_to_exp

>>> square = raise_to(2)
>>> square.__closure__
(<cell at 0x0000023509591BE8: int object at 0x00007FFB067BEF20>,)
>>> square(5)
25
>>> square(9)
81
```

### Nonlocal
`nonlocal` introduces names from the enclosing namespace into the local namespace.

#### Function decorators
Modify or enhance functions without changing their definition. Decorators are implemented as callables that take and return other callables.
* Replace, enhance, or modify existing functions
* Does not change the original function definition
* Calling code does not need to change
* Decorator mechanism uses the modified function's original name

```python
"""
Takes the callable `f` and returns another callable `wrap`
"""

def escape_unicode(f): 
    #              ^-- This is the function to be decorated.
    def wrap(*args, **kwargs):
        x = f(*args, **kwargs)
        return acii(x)

    return wrap

@escape_unicode
def northern_city()
    return 'Tromsø'
```

In classes, the class objects themselves are not decorators. The instances of the class can be used as decorators.


Multiple decorators
```python
@decorator1
@decorator2
@decorator3
def some_function():
    pass

'''Some function is passed through decorator3. Callable returned by decorator3 is passed to decorator2, and then the returned callable is passed to decorator1, which is then bound to some_function().'''
```

#### functools.wraps()
Properly update metadata on wrapped functions. `__name__` and `__doc__` are properly implemented.

```python
import functools

def noop(f):
    @functools.wraps(f)
    def noop_wrapper():
        return f()
    return noop_wrapper

@noop
def hello():
    "Print a well-known message."
    print('Hello, world!')
```

#### Static Methods
Static methods allow you associate methods with the class, rather than with instances of the class.
* No access needed to either <b>class</b> or <b>instance</b> objects.
* Most likely and implementation detail of the class.
* May be able to be moved to become a module-scope function.

```python
class ShippingContainer:
    
    next_serial = 1337

    @staticmethod
    def _get_next_serial():
        # Note the leading underscore, to denote a `private` function.
        result = ShippingContainer.next_serial
        ShippingContainer.next_serial += 1
        return result

    def __init__(self, owner_code, contents):
        self.owner_code = owner_code
        self.contents = contents
        self.serial = ShippingContainer._get_next_serial()
        #                  ^-- As opposed to `self._get_next_serial`
```

#### Class Methods
Requires access to the class object to call other class methods or the constructor.
* Use if you need access to class attributes.

```python
class ShippingContainer:
    
    next_serial = 1337

    @classmethod
    def _get_next_serial(cls):
        #                 ^-- By convention, pass the class object. 
        result = cls.next_serial
        cls.next_serial += 1
        return result
    
    @classmethod:
    def create_empty(cls, owner_code):
        return cls(owner_code, contents=None)

    @classmethod:
    def create_with_items(cls, owner_code, items):
        return cls(owner_code, contents=list(items))

    def __init__(self, owner_code, contents):
        self.owner_code = owner_code
        self.contents = contents
        self.serial = ShippingContainer._get_next_serial()
```

#### Properties
Property decorators can convert methods into something that when accessed, behaves like an attribute. Can be used to call getter methods so that they can be used as attributes.

```python
@property
def celsius(self):
    return self._celsius

>>> r4 = RefrigeratedShippingContainer.create_with_items('YML', ['fish'], celsius=-18.0)
>>> r4.celsius
-18.0
```

```python
@celsius.setter
def celsius(self, value):
    if value > RefrigeratedShippingContainer.MAX_CELSIUS:
        raise ValueError('Temperature too hot!')
    self._celsius = value
```

