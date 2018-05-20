---
layout: notebook
title: "Web Scraping with Selenium!"
date: 2017-10-06
excerpt: Building a Web scrapinger that lets us gadder data from Yahoo
tags: Yahoo Selenium Stocks
---

#### install selenium
pip install selenium 

#### chromedriver
Get it from http://chromedriver.chromium.org/downloads


```python
import os
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as ec
from selenium.webdriver.common.by import By
```


```python
# webdriver
patch = os.getcwd() + "\\" + "chromedriver.exe"
driver = webdriver.Chrome(patch)
```


```python
# load_page
address = 'https://finance.yahoo.com/quote/ERIC-B.ST/history'
driver.get(address)
```


```python
# WebDriverWait with 2 sec delay
wait = WebDriverWait(driver, 2)
```


```python
# xPath
btn_working = '//*[@id="Col1-1-HistoricalDataTable-Proxy"]/section/div[1]/div[2]/span[2]/a/span'
btn_not_working = '//*[@id="Col1-1-HistoricalDataTable-Proxy"]/section/div[1]/div[2]/span[2]/a/'
```


```python
# not-clickable
button = wait.until(ec.element_to_be_clickable((By.XPATH, btn_not_working)))
```


    ---------------------------------------------------------------------------

    TimeoutException                          Traceback (most recent call last)

    <ipython-input-6-d941d8077818> in <module>()
          1 # not-clickable
    ----> 2 button = wait.until(ec.element_to_be_clickable((By.XPATH, btn_not_working)))


    ~\Anaconda3\lib\site-packages\selenium\webdriver\support\wait.py in until(self, method, message)
         78             if time.time() > end_time:
         79                 break
    ---> 80         raise TimeoutException(message, screen, stacktrace)
         81
         82     def until_not(self, method, message=''):


    TimeoutException: Message:




```python
# Add try and catch
from selenium.common.exceptions import TimeoutException
try:
    button = wait.until(ec.element_to_be_clickable((By.XPATH, btn_not_working)))
except TimeoutException as ex:
    error_log = "TimeoutException {0} ".format(ex)
    print(error_log)
```


```python
# clickable
try:
    button = wait.until(ec.element_to_be_clickable((By.XPATH, btn_working)))
except TimeoutException as ex:
    error_log = "TimeoutException {0} ".format(ex)
    print(error_log)
```


```python
button
```


```python
# click
button.click()
```


```python
def time_dec(func):
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

    @time_dec
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
# Yahoo
```


```python
address = 'https://finance.yahoo.com/quote/ERIC-B.ST/history'
```


```python
browser.load_page(address)
```


```python
text = browser.driver.page_source
```


```python
download_btn = browser.driver.find_element_by_xpath('//*[contains(text(), "Download Data")]')
```


```python
download_btn.click()
```


```python
browser.click(download_btn)
```


```python
browser.shutdown()
```
