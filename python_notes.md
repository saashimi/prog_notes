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
Modify or enhance functions without changing their definition.
* Replace, enhance, or modify existing functions
* Does not change the original function definition
* Calling code does not need to change
* Decorator mechanism uses the modified function's original name

In classes, the class objects themselves are not decorators. The instances of the class can be used as decorators.

```python

```

Multiple decorators
```python
@decorator1
@decorator2
@decorator3
def some_function():
    pass

'''Some function is passed through decorator3. Callable returned by decorator3 is passed to decorator2, and then the returned callable is passed to decorator1, which is then bound to some_function().'''
```