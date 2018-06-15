# Stage 2 - CSS에 대해 알아보자

### CSS가 뭔가요?

이전 스테이지에서 HTML에 대해 설명드리면서 코드와 그 결과를 보여드렸습니다.

![](../.gitbook/assets/image%20%2833%29.png)

알기 쉽게 보여드리려고 실제 사진을 가져온 것이

![](../.gitbook/assets/image%20%2812%29.png)

실제로 코드를 실행시켜보면 이렇게 못생긴 페이지가 나온답니다.  
둘 다 흰 바탕에 텍스트 위주라 큰 체감이 느껴지지 않으신다면

![](../.gitbook/assets/image%20%2816%29.png)

웹 개발 스터디에서 잠시 빌려왔습니다.  
왼쪽이 HTML만으로 구성한 페이지이고 오른쪽이 CSS를 추가한 페이지입니다.

사람을 대상으로 만드는 웹페이지이기 때문에 보기 좋아야 하는 것은 물론이고, 이용 편의성과도 연결되니 매우 중요한 부분입니다.

### CSS의 작성 방식

CSS의 작성 방식에는 여러가지가 있지만, 처음엔 HTML 태그에 직접 집어넣는 것이 쉽습니다.

```text
without css code
```

![](../.gitbook/assets/image%20%2814%29.png)

이러한 HTML only 코드에서

```text
with css code
```

![](../.gitbook/assets/image%20%282%29.png)

CSS를 추가한 모습입니다.

src, href등과 같이 태그 내에 style이라는 속성을 추가하여 CSS를 삽입하는 방법입니다.  
삽입되는 태그의 형식은 스타일명: 값;  입니다. 여러 스타일을 동시에 적용할 때는 꼭 ;로 구분해줘야 합니다.

### 몇 가지 CSS 스타일 배워보기

쉽게 적용해 볼 수 있는 몇 가지 스타일을 배워보도록 하겠습니다.

#### color 속성

요소 안에 있는 글자의 색을 바꿉니다.

```text
color sample code
```

color sample image



#### background-color

요소의 배경색을 바꿉니다.

```text
bg sample code
```

bg sample image



{% hint style="info" %}
색에 들어가는 값이 생소하시죠?

웹에서 색을 정하는 원리에 대해 알려드리겠습니다.

기본적으로 빛의 삼원색을 각각 0에서 255까지 지정할 수 있습니다.  
각 색깔은 2자리 16진수로 표현됩니다. 00은 색이 없다는 뜻이고, ff가 가장 강한 색입니다.  
16진수는 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 다음에 a, b, c, d, e, f 가 나오구요.  
앞자리가 뒷자리보다 변동폭이 더 크겠죠?  
0f보다 20이 크다는 뜻입니다. 10진수에서 09보다 20이 큰 것처럼요.

그리고 세가지 색을 빨강, 초록, 파랑의 순서로 합쳐서 \#빨빨초초파파 의 형태로 표현합니다.  
다음 예시를 봐주세요.

![](../.gitbook/assets/image%20%2830%29.png)

\#ff0000은 빨강이 ff로 최대값이고 초록, 파랑은 0입니다. 따라서 새빨간 색이 됩니다.

\#0088cc는 빨강이 0이고 초록은 중간정도, 파랑은 최대값에 가깝습니다. 약간 연한 파랑색이 되었네요.

나머지도 마찬가지로 생각해보세요!  
{% endhint %}



#### margin

구역 바깥에 여백을 줍니다.

```text
margin sample code
```

![&#xAC01; &#xC0C1;&#xC790;&#xAC00; &#xC790;&#xC2E0;&#xC744; &#xB458;&#xB7EC;&#xC2FC; &#xBD80;&#xBAA8; &#xC694;&#xC18C;&#xB098; &#xB2E4;&#xB978; &#xC694;&#xC18C;&#xB4E4;&#xACFC; &#xAC04;&#xACA9;&#xC744; &#xBC8C;&#xB9AC;&#xACE0; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.](../.gitbook/assets/image%20%2818%29.png)



#### padding

구역 안에 여백을 줍니다.

```text
padding sample code
```

![&#xAC01; &#xC0C1;&#xC790;&#xB294; &#xC678;&#xBD80; &#xC694;&#xC18C;&#xB4E4;&#xACFC;&#xB294; &#xC5EC;&#xBC31; &#xC5C6;&#xC774; &#xBD99;&#xC5B4;&#xC788;&#xACE0;, &#xB0B4;&#xBD80;&#xC758; &#xD14D;&#xC2A4;&#xD2B8;&#xC640; &#xAC04;&#xACA9;&#xC744; &#xBC8C;&#xB9AC;&#xACE0; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.](../.gitbook/assets/image%20%2825%29.png)



