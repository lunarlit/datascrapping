# 모범 답안

```python
from selenium import webdriver

driver = webdriver.Chrome('./chromedriver')

# 파파고 접속
driver.get('https://papago.naver.com/')

# 번역할 문장 입력
driver.find_element_by_css_selector('#sourceEditArea textarea').send_keys('i love koala study')

# 번역 버튼 클릭
driver.find_element_by_css_selector('#btnTranslate').click()
```

