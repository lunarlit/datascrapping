# Stage 4 - 원하는 데이터를 찾아가보자

이번 스테이지에서는 크롬 브라우저의 사용을 권장합니다. 훌륭한 개발자 도구를 갖추고 있어 큰 도움이 됩니다.

### 원하는 요소를 직접 찾아가보자

![](../.gitbook/assets/image%20%286%29.png)

2주차에는 네이버TV TOP100의 클립들을 수집하여 프로그램별로 재정리합니다.  
이를 위해 클립명과 조회수의 경로를 미리 찾아가 보겠습니다.

1. 크롬 브라우저를 열고 원하는 사이트로 이동합니다.
2. F12를 누릅니다.
3. 개발자 도구가 열리면 탭이 Elements에 가있는지 확인합니다.

![](../.gitbook/assets/image%20%2810%29.png)

아래에 열린 개발자 도구를 보면 그 동안 봐왔던 HTML 태그를 직접 확인할 수 있습니다!

4. 요소에 마우스를 가져다 대봅니다.

![](../.gitbook/assets/image%20%2824%29.png)

div\#wrap에 마우스를 대 보았더니, 브라우저 창에 색이 입혀지는 것을 볼 수 있습니다.

이는 지금 대고 있는 요소의 범위를 나타냅니다. 녹색은 padding을 나타내고, 파란색 영역이 실제 요소의 내용입니다.

범위가 너무 넓으니 div\#wrap 좌측의 세모 버튼을 눌러 내부로 더 들어가 봅시다.

![](../.gitbook/assets/image%20%2829%29.png)

div\#wrap의 내부입니다.

script 태그는 HTML 요소가 아니므로 무시하셔도 좋으며, 나머지는 각각 div\#header, div\#aside, div\#container, div.floating\_top 입니다. 각 div에 다시 마우스를 대보면 div\#wrap의 구조를 더 잘 파악할 수 있습니다.

div separated image

우리가 원하는 클립 리스트는 \#container 안에 있네요. 한 번 더 펼쳐봅시다.

![](../.gitbook/assets/image%20%2839%29.png)

div.top100이라는 자식 요소 안에 또 다시 div.cate\_tit2, div.top\_main, div\#content가 있네요.

최상위 3개 클립은 div.top\_main에, 나머지 97개는 div\#content에 들어있는 것을 확인할 수 있습니다.

모두 수집하려면 둘 다 처리해야겠네요. 지금은 자료가 더 많은 div\#content로 들어가봅시다.

![](../.gitbook/assets/image%20%2819%29.png)

div.top100\_mv와 div.wrp\_cds를 거치면 그 안에 수많은 div.cds가 들어있고, 마우스를 대보면 이 div 하나하나가 각 클립을 포함하고 있다는 것을 알 수 있습니다.

이렇게 리스트의 각 요소까지 접근했다면 성공적으로 수집할 수 있다고 보시면 됩니다.

남은 것은 이 요소를 더 파고들어 원하는 정보만 뽑아내는 것입니다.

![](../.gitbook/assets/image%20%283%29.png)

내부를 펼쳐보았습니다. 차분히 따라가보면

썸네일 사진을 담당하는 a.cds\_thm  
순위를 알리는 span.num  
순위 변동을 알리는 span.rank   
기타 정보들이 들어있는 dl.cds\_info 네 부분으로 나눠진 것을 알 수 있습니다.

우리가 원하는 정보들은 모두 dl.cds\_info에 들어있는 상황입니다.

![](../.gitbook/assets/image%20%288%29.png)

마지막으로 dl.cds\_info를 모 펼쳐보았습니다.

클립 제목을 나타내는 dt.title  
채널\(프로그램명\)을 나타내는 dd.chn  
조회수, 좋아요 수가 들어있는 dd.meta 가 있습니다.

처음부터 따라가보면,   
클립정보를 담은 div의 위치는 div\#wrap &gt; div\#container &gt; div.top100 &gt; div\#content &gt; div.top100\_mv &gt; div.wrp\_cds &gt; div.cds\_area &gt; div.cds 입니다.

여기서 클립 제목을 가져오려면 추가로  
dl.cds\_info &gt; dt.title &gt; a &gt; tooltip 으로 접근하면 되고

조회수를 가져오려면  
dd.meta &gt; span.hit 으로 접근하면 됩니다.

너무 길죠?  
먼저 경로의 div\#content 전까지는 생략하여도 ID로 항상 div\#content 찾을 수 있기 때문에 생략합니다.

이제 클립 제목에 도달하는 경로는  
div\#content &gt; div.top100\_mv &gt; div.wrp\_cds &gt; div.cds\_area &gt; div.cds &gt; dl.cds\_info &gt; dt.title &gt; a &gt; tooltip 입니다.

더 줄여봅시다.  
클래스를 생략할 수 없는 이유는 정확성이 보장되지 않기 때문이라고 말씀드렸습니다.  
각 클립은 div.cds로 감싸져있어 모든 div.cds를 가져오면 모든 클립을 가져올 수 있지만, 혹시 다른 곳에 사용된 div.cds까지 가져올 수 있기 때문이죠.  
이를 직접 해결하려면 직접 찾아보면 됩니다.

![](../.gitbook/assets/image%20%281%29.png)

개발자 도구에서 Ctrl + F를 누르면 검색이 가능합니다. 여기서 div.cds를 검색하여 클립 정보 이외에 사용된 적이 있는지 찾아봅니다.

모두 찾아본 결과 div.cds는 클립 정보만 포함하고 있다는 결론을 내렸습니다. 이제 div.cds 이전의 경로를 모두 생략합니다.

이제 클립 제목의 경로는 div.cds &gt; dl.cds\_info &gt; dt.title &gt; a &gt; tooltip입니다.  
처음에 비하면 정말 많이 줄었네요. 여기서 더 줄일 수 있을까요?

구조를 보면 div.cds 안에 dt.title은 하나밖에 없습니다. 따라서 dl.cds\_info를 생략해도 정확성이 유지됩니다.   
다음으로, dt.title 안에 tooltip은 하나밖에 없습니다. 따라서 a를 생략해도 정확성이 유지됩니다.

이렇게 중간 자식 요소를 건너 뛸 때에는 자식 선택자가 아닌 자손 선택자를 사용하여야 합니다.  
결론적으로 제가 찾은 클립 제목의 최종 경로는div.cds dt.title tooltip 입니다.



