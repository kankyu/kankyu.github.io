---
layout: post
title:  Interfaces and Protocols
date:   2018-10-06 
categories: ["Object Oriented Programming", "Python"]
---

### Interface

An interface is a programming structure that allowed us to enforce certain properties on an object/ class. E.g. say we have a car class and scooter class and a truck class. Each of the three classes should have a start_engine() method.

### Duck Typing

In runtime invoke methods we expect the method to have (occurs at runtime).

> For safety checks we use the try except approach or the hasattr.

We often hear about file like object or an iterable. If an object has a `read` method, it can be treated as a file like object., if the object has an `__iter__` magic method, it is an iterable. So any object can conform to a certain interface just by implementing the expected behaviour (methods).

### Magic methods

Magic methods belonging to a class, is an informal interface (to enforce certain properties on an object or class). It makes the object conform to protocols. We can implement a protocol by implementing the methods expected by it.

> You can think of protocols as enforced properties for an object / class.

```python
class Team:
    def __init__(self, members):
        self.__members = members
    
    def __len__(self):
        return len(self.__members)
        
    def __contains__(self, member):
        return member in self.__members
        
    def __iter__(self):
        for member in self.__members:
            yield member # the yield allows the object to become an iterable.
        
justice_league = Team(["batman", "wonder women", "flash"])

# Sized protocol
print(len(justice_league))

# Container protocol
print("batman" in justice_league)

print(type(justice_league))

for member in justice_league:
    print(member)
```

    3
    True
    <class '__main__.Team'>
    batman
    wonder women
    flash

### Formal interfaces ABCS

Informal interfaces or duck typing can cause confusion. For example, `Bird` and `Aeroplane` both can `fly()`. But they are not the same, even if they implement the same (informal) interface / protocols. Abstract Base Classes (ABCs) can help solve this issue.

Any objects deriving from these base classes are **forced to implement those methods**. This is equivalent to a interface.

If helps us enforce other developers to realise when deriving from this "interface" they are made to define the necessary methods. This is especially useful for frameworks or you want to make sure rules are being established.

#### Benefit of interfaces

* enforce behaviour for other developers to follow
* catch misspelled or forgotten methods to catch bugs.

```python
import abc

class Bird(abc.ABC):
    @abc.abstractmethod
    def fly(self):
        pass
    
# now if any class derives from our base `Bird` class, it must implement the `fly` method too.
```

```python
class Parrot(Bird):
    pass

p = Parrot()
```

    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-20-d76f7476ac6d> in <module>()
          2     pass
          3 
    ----> 4 p = Parrot()

    TypeError: Can't instantiate abstract class Parrot with abstract methods fly

```python
class Parrot(Bird):
    def fly(self):
        pass

p = Parrot()

print(isinstance(p, Bird)) # parrot class is recognized as an instance of Bird ABC`
```

    True

```python
class Aeroplane(abc.ABC):
    @abc.abstractmethod
    def fly(self):
        pass
        
class Boeing(Aeroplane):
    def fly(self):
        pass
        
b = Boeing()

print(isinstance(p, Aeroplane))
print(isinstance(b, Bird))
```

    False
    False

You can see that even though objects have the same methods, we can tell the difference between the Aeroplane interface and Boeing interface.

**It is often discouraged to create custom ABCs**, but instead use the build-in ones. Before writing you own ABC check if there's an ABC for the same purpose in the standard library 

[https://docs.python.org/3/library/collections.abc.html#module-collections.abc](https://docs.python.org/3/library/collections.abc.html#module-collections.abc)
