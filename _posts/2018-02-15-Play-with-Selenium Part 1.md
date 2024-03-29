---
layout: notebook
title: "Play with Selenium Part 1"
date: 2018-02-15
excerpt: First part, a fast walk through of selenium with a goal to click on a object.
tags: Python Selenium  
---


There are two things we need to have before we create some awesome web scraper. A driver, in this case, I’ll be using [chromedriver](http://chromedriver.chromium.org/downloads) and the library [selenium](https://pypi.org/project/selenium/). The goal with this notebook is to build a browser that lets us click.


```python
import os
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as ec
from selenium.webdriver.common.by import By
```

Select the path to the chromedriver.exe file this will create the driver and it will load the browser. In my case the path is in the current working directory.


```python
# webdriver
patch = os.getcwd() + "\\" + "chromedriver.exe"
driver = webdriver.Chrome(patch)
```

The driver has some in build functions as .get(), and if we input an address, the browser will load that page.


```python
# load_page
address = 'https://www.washingtonpost.com/'
driver.get(address)
```

There's also one crucial future, selenium supports "loading time" and can wait until a page/element is loaded. Here we select a waiting time of two seconds.

```python
# WebDriverWait with 2 sec delay
wait = WebDriverWait(driver, 2)
```

To test this feature, we will be trying to click on a bottom that exists and one that doesn’t. We will notice that the one that doesn’t exist will return a timeout exception.


```python
# xPath
xpath_working = '//*[@id="section-menu-btn"]'
xpath_notworking = '//*[@id="section-menu-btn"]sdadsadw'
```


```python
# not-clickable
button = wait.until(ec.element_to_be_clickable((By.XPATH, xpath_notworking)))
```


    ---------------------------------------------------------------------------

    TimeoutException                          Traceback (most recent call last)

    <ipython-input-6-1e255f4f28f8> in <module>()
          1 # not-clickable
    ----> 2 button = wait.until(ec.element_to_be_clickable((By.XPATH, xpath_notworking)))


    ~\Anaconda3\lib\site-packages\selenium\webdriver\support\wait.py in until(self, method, message)
         78             if time.time() > end_time:
         79                 break
    ---> 80         raise TimeoutException(message, screen, stacktrace)
         81
         82     def until_not(self, method, message=''):


    TimeoutException: Message:



So we surround it with a try/except.


```python
# Not working
from selenium.common.exceptions import TimeoutException
try:
    button = wait.until(ec.element_to_be_clickable((By.XPATH, xpath_notworking)))
except TimeoutException as ex:
    error_log = "TimeoutException {0} ".format(ex)
    print(error_log)
```

    TimeoutException Message:




```python
# Working
try:
    button = wait.until(ec.element_to_be_clickable((By.XPATH, xpath_working)))
except TimeoutException as ex:
    error_log = "TimeoutException {0} ".format(ex)
    print(error_log)
```


```python
# click
button.click()
```


```python
# Shutdown
driver.close()
```


In the [next part]({% post_url  2018-02-20-Play-with-Selenium Part 2%}) I'll create a class to make it more beautiful and easy to control. We will surround it with a decorator, and if you are new to decorators, I have a short [example]({% post_url 2018-01-26-Decorator %}) on that too.
