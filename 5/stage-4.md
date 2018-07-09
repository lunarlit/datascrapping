# Stage 4 - 실습 예제 : 게시판 안으로 들어가기

이제 데이터수집에 필요한 대부분의 기능을 익히셨습니다. 

아직 다루지 않은 기능이 있더라도 기존 형태에서 크게 벗어나지 않으므로 검색이나 질문을 통해 응용할 수 있을 것이라 생각합니다.

마지막으로 리스트 형태의 게시판이 있을 때, 리스트의 각 링크를 조회하여 내부 정보까지 수집하는 방법을 알아보겠습니다. 꽤 자주 볼 수 있는 수집 구조입니다.



## 상세 페이지 조회

![](../.gitbook/assets/image%20%28162%29.png)

네이버 지도 예제를 계속 사용하도록 하겠습니다.

상세보기라는 버튼을 누르면 리스트에는 나오지 않는 여러가지 정보가 추가로 나옵니다.

데이터 수집을 하다보면 이런 데이터까지 접근해야 할 일이 생각보다 자주 있는데요.

그 방법을 한번 배워보도록 하겠습니다.



### 1. 버튼 직접 누르기

가장 먼저, 상세보기를 누르려면 어떻게 해야 할까요?

현재 브라우저를 조종하고 있으니, 상세보기 버튼을 찾아 눌러보겠습니다.

```python
list = driver.find_elements_by_css_selector('ul.lst_site > li')

for data in list:
    data.find_element_by_css_selector('li.detail > a').click()
```

리스트를 찾아 list라는 변수에 배열로 집어넣고, for문을 돌며 각 리스트 요소에서 상세보기 버튼의 경로로 선택하여 클릭을 명령하고 있습니다.

![](../.gitbook/assets/image%20%2899%29.png)

하지만 이런 에러를 마주하게 될 것입니다.

ElementNotVisibleException. 요소가 보이지 않는다는 뜻입니다.

Stage 3에서 보았던 NoSuchElementException과는 조금 다른데요.

ElementNotVisibleException은 요소가 존재하긴 하지만, 가려져있어 선택할 수 없다는 뜻입니다.

![](../.gitbook/assets/image%20%281%29.png)

브라우저를 보면 해당 리스트를 클릭하여 선택해야만 상세보기 버튼이 나오는 것을 알 수 있습니다. 가상브라우저는 리스트를 클릭하라는 명령을 받지 못했기 때문에 상세보기 버튼을 찾을 수 없는 것이죠.



```python
list = driver.find_elements_by_css_selector('ul.lst_site > li')

for data in list:
    data.find_element_by_css_selector('dl.lsnx_det > dt > a').click()
    data.find_element_by_css_selector('li.detail > a').click()
```

그럼 이렇게 먼저 리스트 제목을 누른 후 상세보기를 누르게 하면 되지 않을까요?

첫번째 상세보기까지는 잘 눌리지만, 또다른 난관에 봉착합니다.

![](../.gitbook/assets/image%20%28136%29.png)

바로 탭이 생기는 것인데요. 인간이 직접 사용할 때에는 자연스럽게 정보를 확인하고, 탭을 닫고, 리스트로 돌아가 다음 요소를 눌러볼 수 있습니다.

하지만 코드로 브라우저를 다룰때는 그리 간단한 일이 아닙니다. 탭을 닫고 메인 탭으로 이동하는 방법을 새로 배워야 합니다.



```python
list = driver.find_elements_by_css_selector('ul.lst_site > li')

for data in list:
    data.find_element_by_css_selector('dl.lsnx_det > dt > a').click()
    data.find_element_by_css_selector('li.detail > a').click()
    driver.switch_to.window(driver.window_handles[1])
    # 상세 데이터 수집 생략...
    
    driver.close()
    driver.switch_to.window(driver.window_handles[0])
```

드라이버 변수의 switch\_to.window\( \) 함수를 사용하면 조종중인 브라우저의 탭을 바꿀 수 있습니다. 매개변수는 driver.window\_handles 배열의 요소 하나입니다.

driver.window\_handles는 순서대로 각각의 탭을 가리키고 있는 배열입니다. 즉 현재 열린 상세 페이지의 번호는 1번입니다.

탭 이동 후 상세 데이터를 수집하고나면, 탭을 닫고 맨 처음의 driver.window\_handles\[0\] 으로 돌아와야 합니다.

이 코드를 이용하면 인간이 하는 것과 똑같이 새 탭을 열고 닫으며 수집할 수 있습니다.



### 2. 링크 주소로 이동

상세보기 버튼은 아래와 같은 코드를 가지고 있습니다.

```markup
<a href="/local/siteview.nhn?code=33546222" 
    target="_blank" class="spm spm_sw_detail _qbtn_detail nclicks(plc.detail)" 
    data-id="s33546222">
    상세보기
</a>
```

주소를 눌러 이동하는 a태그이고, target="\_blank" 라는 속성을 주어 새 탭에서 열리도록 지정한 모습입니다.

이 주소를 획득하여 버튼을 누르지 않고 직접 주소로 이동하는 방법도 있습니다.

{% hint style="info" %}
a가 가지고 있는 주소 형태는 전체 URL이 아니라 https://map.naver.com에서 시작하는 부분 URL 입니다.

따라서 해당 주소로 접속하려면 앞에 https://map.naver.com을 꼭 붙여주어야 합니다!
{% endhint %}



BeautifulSoup과 selenium 모두 선택된 HTML 요소의 속성값을 얻어오는 방법을 가지고 있습니다.

| selenium | .get\_attribute\('속성명'\) |
| --- | --- |
| BeautifulSoup | .attrs\['속성명'\] |



```python
# BeautifulSoup 사용시
link = data.select_one('li.detail > a').attrs['href']

# selenium 사용시
#link = data.find_element_by_css_selector('li.detail > a').get_attribute('href')

driver.get('https://map.naver.com' + link)
```

이런 방식으로 상세 주소에 드라이버를 접속시킬 수 있습니다.



