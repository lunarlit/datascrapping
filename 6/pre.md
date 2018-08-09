# Pre - 실습 코드 데이터



## Page 5 - 수집 대상 페이지에 접속

```python
import requests
from bs4 import BeautifulSoup

req = requests.get('https://search.shopping.naver.com/search/category.nhn?cat_id=50001203&pagingSize=40&pagingIndex=1')
raw = req.text
html = BeautifulSoup(raw, 'html.parser')
```



## Page 6 - 리스트 가져오기

```python
items = html.select('li._itemSection')
```



## Page 7 - 리스트 요소에서 데이터 추출

```python
for item in items:
    rank = int(item.attrs['data-expose-rank'])
    info = item.select_one('div.info')

    name = info.select_one('a.tit').text
    price = info.select_one('span.price span.num')
    reload = price.attrs['data-reload-date']
    price = price.text

    try:
        star = info.select_one('span.etc span.star_graph span').attrs['style']
    except:
        star = '-'

    comments = info.select_one('span.etc em').text
    brand = item.select_one('div.info_mall > p.mall_txt > a.mall_img')

    try:
        brand = brand.attrs['title']
    except:
        brand = brand.text

    print(rank, name, price, reload, star, comments, brand)
```

## Page 8 - 추출한 데이터 가공

```python
for item in items:
    rank = int(item.attrs['data-expose-rank'])
    info = item.select_one('div.info')

    name = info.select_one('a.tit').text.strip()
    price = info.select_one('span.price span.num')
    reload = price.attrs['data-reload-date']
    price = price.text

    try:
        star = info.select_one('span.etc span.star_graph span').attrs['style'][6:]
    except:
        star = '-'

    comments = info.select_one('span.etc em').text
    brand = item.select_one('div.info_mall > p.mall_txt > a.mall_img')

    try:
        brand = brand.attrs['title']
    except:
        brand = brand.text

    brand = brand.split(' ')[0]

    print(rank, name, price, reload, star, comments, brand)
```

## Page 15 - 데이터 축적

```python
companyItems = {}
```

```python
if brand in companyItems.keys():
    companyItems[brand].append({
        'rank': rank, 'name': name, 'price': price, 'reload': reload, 'star': star, 'comments': comments, 'brand': brand
    })
else:
    companyItems[brand] = [{
        'rank': rank, 'name': name, 'price': price, 'reload': reload, 'star': star, 'comments': comments, 'brand': brand
    }]
```



## Page 16 - 웹페이지 반복

```python
for page in range(1, 10):
    req = requests.get('https://search.shopping.naver.com/search/category.nhn?cat_id=50001203&pagingSize=40&pagingIndex=' + str(page))
```



## Page 17 - 엑셀에 저장

```python
import openpyxl

wb = openpyxl.Workbook()
ws = wb.active
```

```python
ws.append(['제조사', '순위', '품명', '최저가', '가격 갱신일', '만족도', '후기 수'])

for companyItem in companyItems.items():
    ws.append([companyItem[0]])

    for item in companyItem[1]:
        ws.append(['', item['rank'], item['name'], item['price'], item['reload'], item['star'], item['comments']])

    ws.append([])


wb.save('mouse.xlsx')
```



## Page 22 - Stage 3 시작 코드

```python
import requests
from bs4 import BeautifulSoup
from selenium import webdriver
import time

req = requests.get('https://news.ycombinator.com/news?p=1')
html = BeautifulSoup(req.text, 'html.parser')

titles = html.select('a.storylink')

for title in titles:
    print(title.text)
```



## Page 23 - 페이지 접속 & 기능 조작

```python
driver = webdriver.Chrome('./chromedriver')
driver.get('https://papago.naver.com/')
```

```python
driver.find_element_by_css_selector('textarea#txtSource').send_keys(title.text)
driver.find_element_by_css_selector('button#btnTranslate').click()
time.sleep(1)
translated = driver.find_element_by_css_selector('div#txtTarget').text
print(translated)
driver.find_element_by_css_selector('textarea#txtSource').clear()
time.sleep(1)
```



## Page 25 - 정적 페이지 반복

```python
import requests
from bs4 import BeautifulSoup
from selenium import webdriver
import time

engKor = []

driver = webdriver.Chrome('./chromedriver')

for n in range(1, 5):

    req = requests.get('https://news.ycombinator.com/news?p=' + str(n))
    html = BeautifulSoup(req.text, 'html.parser')

    titles = html.select('a.storylink')

    driver.get('https://papago.naver.com/')


    for title in titles:
        orig = title.text
        driver.find_element_by_css_selector('textarea#txtSource').send_keys(title.text)
        driver.find_element_by_css_selector('button#btnTranslate').click()
        time.sleep(1)
        translated = driver.find_element_by_css_selector('div#txtTarget').text
        # print(orig, translated)
        engKor.append([orig, translated])
        driver.find_element_by_css_selector('textarea#txtSource').clear()
```



## Page 25 - 멀티 쓰레드

```python
import requests
from bs4 import BeautifulSoup
from selenium import webdriver
import threading

engKor = []

def processMany(start, end):
    driver = webdriver.Chrome('./chromedriver')

    for n in range(start, end + 1):

        req = requests.get('https://news.ycombinator.com/news?p=' + str(n))
        html = BeautifulSoup(req.text, 'html.parser')

        titles = html.select('a.storylink')

        driver.get('https://papago.naver.com/')


        for title in titles:
            orig = title.text
            driver.find_element_by_css_selector('textarea#txtSource').send_keys(title.text)
            driver.find_element_by_css_selector('button#btnTranslate').click()
            time.sleep(1)
            translated = driver.find_element_by_css_selector('div#txtTarget').text
            # print(orig, translated)
            engKor.append([orig, translated])
            driver.find_element_by_css_selector('textarea#txtSource').clear()

t1 = threading.Thread(target=processMany, args=(1, 2))
t2 = threading.Thread(target=processMany, args=(3, 4))
t3 = threading.Thread(target=processMany, args=(5, 6))

t1.start()
t2.start()
t3.start()

t1.join()
t2.join()
t3.join()

print(engKor)
```

