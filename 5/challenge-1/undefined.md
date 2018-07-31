# 모범 답안



```python
from selenium import webdriver

driver = webdriver.Chrome('./chromedriver')

driver.get('https://www.naver.com')

driver.find_element_by_id('id').send_keys('doomsdt')
driver.find_element_by_id('pw').send_keys('5891421lL!')
driver.find_element_by_css_selector('span.btn_login').click()

driver.find_element_by_css_selector('a.mn_mail').click()
```

메일함으로 접근하는 방법은 여러가지가 있습니다. 위 코드에서는 상단 메뉴의 메일 버튼을 누릅니다.

![](../../.gitbook/assets/image%20%2823%29.png)

