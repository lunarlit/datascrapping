# 모범 답안

```python
import requests
from selenium import webdriver
from selenium import common
from bs4 import BeautifulSoup
import time

driver = webdriver.Chrome('./chromedriver')
driver.get('https://datalab.naver.com/keyword/realtimeList.naver?where=main')

driver.find_element_by_css_selector('div.time_indo span.on').click()
driver.find_element_by_css_selector('div.hour > input._hour').clear()
driver.find_element_by_css_selector('div.hour > input._hour').send_keys('10')
driver.find_element_by_css_selector('div.minute > input._minute').clear()
driver.find_element_by_css_selector('div.minute > input._minute').send_keys('00')
driver.find_element_by_css_selector('div.sec > input._second').clear()
driver.find_element_by_css_selector('div.sec > input._second').send_keys('00')
time.sleep(1)
driver.find_element_by_css_selector('.layer_time > div.sub_area.v2 > a.btn_check._confirm').click()

driver.find_element_by_class_name('select_date')

list = driver.find_elements_by_css_selector('div.select_date span.title')

for keyword in list:
    print(keyword.text)
```

