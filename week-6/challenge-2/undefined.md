# 모범 답안

```python
from selenium import webdriver

driver = webdriver.Chrome('./chromedriver')
driver.get('https://datalab.naver.com/keyword/realtimeList.naver?where=main')

driver.find_element_by_class_name('select_date')

list = driver.find_elements_by_css_selector('div.select_date span.title')

for keyword in list:
    print(keyword.text)
```

