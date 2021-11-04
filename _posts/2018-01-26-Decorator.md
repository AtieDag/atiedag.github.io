---
layout: notebook
title:  "Python Decorators"
date: 2018-01-26
excerpt:  I'll walk through some example where decorator can is been used.
tags: Python Decorator
---

By using the properties of nested functions, you can surround a function with another function in so-called wrapping. I'll walk through some examples where this can be helpful.


#### Example 1
We create a function that sleeps for one second, and we wish to time it. 


```python
import time

def my_func(n):
    time.sleep(1)
```


```python
# This is the simplest way

start = time.time()
my_func(1)
end = time.time()
print('Took:', end - start)
```

    Took: 1.0006980895996094





```python
def decorator_func(func):

    def wrapper(s):
        start = time.time()
        func(s)
        end = time.time()
        print('Took:', end - start)

    return wrapper

dec = decorator_func(my_func)
```


```python
dec(1)
```

    Took: 1.0018470287322998


### Syntax
Python allows you to call the decorator using the @ symbol, which works inside a class object.


```python
@decorator_func
def my_func(n):
    time.sleep(1)
```


```python
my_func(3)
```

    Took: 1.0009677410125732


#### Example 2
We create another function with the same name that adds one to the input value.


```python
def decorator_func(func):

    def wrapper(s):
        print('Before')
        func(s)
        print('After')

    return wrapper

dec = decorator_func(my_func)
```


```python
@decorator_func
def my_func(n):
    print(n+1)
```


```python
my_func(2)
```

    Before
    3
    After



```python
@decorator_func
@decorator_func
def my_func(n):
    print(n+1)
```


```python
my_func(3)
```

    Before
    Before
    4
    After
    After



More useful decorators at [Python Decorator Library](https://wiki.python.org/moin/PythonDecoratorLibrary)
