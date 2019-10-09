# Memory Reduction

## Using \_\_slots\_\_

Use slots to reference a list of members and watch the memory usage drop! This also removes `__init__` from the class instances so \*beware\*.

```python
import sys


class Person(object):
    def __init__(self, name, age, address, country):
        self._name = name
        self._age = age
        self._address = address
        self._country = country


class Person2(object):
    __slots__ = ["_name", "_age", "_address", "_country"]

    def __init__(self, name, age, address, country):
        self._name = name
        self._age = age
        self._address = address
        self._country = country


if __name__ == "__main__":
    p1 = Person("Joe Bloggs", 50, "29 Somewhere Street", "United Kingdom")
    size = sys.getsizeof(p1) + sys.getsizeof(p1.__dict__)
    print("p1 size is:", size)

    p2 = Person2("Joe Bloggs", 50, "29 Somewhere Street", "United Kingdom")
    size = sys.getsizeof(p2)
    print("p2 size is:", size)


```

{% hint style="info" %}
 Try running this in python2 and python3
{% endhint %}





