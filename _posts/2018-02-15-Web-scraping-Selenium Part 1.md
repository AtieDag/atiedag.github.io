---
layout: notebook
title: "Web Scraping with Selenium Part 1"
date: 2018-02-15
excerpt: First part is to building a web scrapinger that lets us click around.
tags: Python Selenium  
---


There's two things we need to have before we create some awesome web scraper. A driver, in this case I'll be using [chromedriver](http://chromedriver.chromium.org/downloads) and the libary [selenium](https://pypi.org/project/selenium/). The goal with this notebook is to building a browser that lets us click.


```python
import os
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as ec
from selenium.webdriver.common.by import By
```

Select the path to the chromedriver.exe file this will create the driver and it will load the "fake" browser.


```python
# webdriver
patch = os.getcwd() + "\\" + "chromedriver.exe"
driver = webdriver.Chrome(patch)
```

The driver have some in build functions as .get(), if you input the address it will load that page in the browser.


```python
# load_page
address = 'https://www.washingtonpost.com/'
driver.get(address)
```

There's also one important future, selenium supports "loading time" and can wait until a page/element is loaded.

```python
# WebDriverWait with 2 sec delay
wait = WebDriverWait(driver, 2)
```

To test this feature we will be trying to click on a bottom that exist and one that doesn’t. We will notice that the one that doesn’t exist will return a timeout exception.


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



We surround it with a try/except


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


In the [next part]({% post_url  2018-02-20-Web-scraping-Selenium Part 2%}) I'll create a class to make it more beautiful and easy to control. We will surround it with a decorator and if you are new to decorator I have a short [example]({% post_url 2017-09-06-Decorator %}) on that too.
