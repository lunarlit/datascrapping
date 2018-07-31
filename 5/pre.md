# Pre - 실습 코드 데이터

## Stage 2 - 15p

```python
from selenium import webdriver
from bs4 import BeautifulSoup
import time


driver = webdriver.Chrome('./chromedriver')


# ---- 페이지 접속 ----

driver.get('https://map.naver.com/')


# ---- 요소 탐색, 키보드 입력 ----

driver.find_element_by_id('search-input').send_keys('신촌 스터디룸')


# ---- 요소 탐색, 마우스 클릭 ----

driver.find_element_by_css_selector('#header > div.sch > fieldset > button').click()


# ---- 페이지 수집 ----

list = driver.find_elements_by_css_selector('.lsnx_det')

for data in list:
    print(data.find_element_by_tag_name('a').text)


# ---- 정적 수집으로 전환 ----

# html = driver.page_source
# soup = BeautifulSoup(html, 'html.parser')

# list = soup.select('.lsnx_det')
#
# for data in list:
#
#     try:
#         title = data.select_one('a').text
#         addr = data.select_one('.addr').text[:-3].strip()
#         tel = data.select_one('.tel').text.strip()
#
#     except:
#         tel = ''
#
#     finally:
#         print(title, '/', addr, '/', tel)
#


# ---- 페이지 버튼 클릭하여 넘기기 ----

# driver.find_element_by_xpath('//a[text()=' + str(2) + ']').click()
# time.sleep(1)


# driver.find_element_by_xpath('//a[text()="3"]').click()
# driver.close()
```



## Stage 3

```python
from selenium import webdriver
from selenium import common
from bs4 import BeautifulSoup
import time

driver = webdriver.Chrome('./chromedriver')

driver.get('https://map.naver.com/')

driver.find_element_by_id('search-input').send_keys('신촌 스터디룸')

driver.find_element_by_css_selector('#header > div.sch > fieldset > button').click()

page = 1

while True:
    html = driver.page_source
    soup = BeautifulSoup(html, 'html.parser')

    list = soup.select('.lsnx_det')

    for data in list:

        try:
            title = data.select_one('a').text
            addr = data.select_one('.addr').text.replace('지번', '').strip()
            tel = data.select_one('.tel').text.strip()

        except:
            tel = ''

        finally:
            print(title, '/', addr, '/', tel)

    page = page + 1

    try:
        if page % 5 == 1:
            driver.find_element_by_class_name('next').click()
        else:
            driver.find_element_by_xpath('//a[text()=' + str(page) + ']').click()

    except common.exceptions.NoSuchElementException:
        break

    time.sleep(1)
```



## Stage 4

```python
from selenium import webdriver
from selenium import common
from bs4 import BeautifulSoup
import time

driver = webdriver.Chrome('./chromedriver')
driver2 = webdriver.Chrome('./chromedriver')

driver.get('https://map.naver.com/')

driver.find_element_by_id('search-input').send_keys('신촌 스터디룸')

driver.find_element_by_css_selector('#header > div.sch > fieldset > button').click()

page = 1

while True:
    html = driver.page_source
    soup = BeautifulSoup(html, 'html.parser')

    list = soup.select('ul.lst_site > li')

    for data in list:

        title = data.select_one('a').text

        print(title)

        detail_url = data.select_one('a.spm_sw_detail').attrs['href']
        driver2.get('https://map.naver.com' + detail_url)
        times = driver2.find_elements_by_css_selector('.section_detail_time li')
        for t in times:
            print(t.text)

        print('---------------')
        time.sleep(1)

    page = page + 1

    try:
        if page % 5 == 1:
            driver.find_element_by_class_name('next').click()
        else:
            driver.find_element_by_xpath('//a[text()=' + str(page) + ']').click()

    except common.exceptions.NoSuchElementException:
        break

    time.sleep(1)
```

