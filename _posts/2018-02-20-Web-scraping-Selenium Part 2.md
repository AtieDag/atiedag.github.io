---
layout: notebook
title: "Web Scraping with Selenium Part 2"
date: 2018-02-20
excerpt: Second part is to make the code more clean and eassy to work with.
tags: Python Selenium  
---


```python
import os
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as ec
from selenium.webdriver.common.by import By
from selenium.common.exceptions import TimeoutException
```


```python
def try_dec(func):
    def wrapper(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except TimeoutException as ex:
            error_log = "Error in function: {0} with args:{1} \n{2}"            
            print(error_log.format(func.__name__, args[1], ex))
            return -1
    return wrapper


class Browser:

    def __init__(self, delay=2):        
        patch = os.getcwd() + "\\" + "chromedriver.exe"
        self.driver = webdriver.Chrome(patch)
        self.set_delay(delay)

    def load_page(self, address):
        self.driver.get(address)

    def set_delay(self, delay=2):
        self.wait = WebDriverWait(self.driver, delay)

    def wait_xpath(self, xpath):
        element = self.wait.until(ec.element_to_be_clickable((By.XPATH, xpath)))
        return element

    @try_dec
    def click(self, btn):
        button = self.wait_xpath(btn)
        button.click()  

    def shutdown(self):
        self.driver.close()
```


```python
browser = Browser()
```


```python
browser.load_page('https://www.washingtonpost.com')
```


```python
xpath_section_btn = '//*[@id="section-menu-btn"]'
```


```python
browser.click(xpath_section_btn)
```


```python
browser.shutdown()
```
