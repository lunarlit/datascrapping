# 모범 답안

## 1.  [https://news.ycombinator.com](https://news.ycombinator.com/)[/](https://news.ycombinator.com/) 기사 제목

모범 답안 : table.itemlist tr.athing &gt; a.storylink

경로가 다소 달라도 a.storylink로 끝나면 괜찮습니다. 다만 tr\#17315494 등 기사 ID가 들어가면 안됩니다. 이 경우 기사 하나의 제목밖에 가져오지 못하기 때문에 데이터 수집의 의미가 사라집니다.

## 2.  [http://tech.kakao.com](http://tech.kakao.com/)[/](http://tech.kakao.com/) 태그명

모범 답안 : ul\#post-list &gt; li.post-item &gt; p.post-tags &gt; a.tag

경로가 다소 달라도 a.tag로 끝나면 괜찮습니다. 다만 마지막에 a.tag-reactor 등 태그에 따라 달라지는 클래스가 들어가면 안됩니다. 모든 태그를 수집하는 것이 목표이기 때문입니다.

## 3.  [http://](http://finance.naver.com/sise/lastsearch2.nhn)[finance.naver.com/sise/lastsearch2.nhn](http://finance.naver.com/sise/lastsearch2.nhn) 주식 종목명

모범 답안 : div.box\_type\_l &gt; table &gt; tbody a.tltle

경로가 다소 달라도 a.tltle로 끝나면 괜찮습니다. 함정은 title이 아니라 tltle 인 것입니다. 치사하지만 실제로도 오타가 있는 문서에서 수집해야 하는 경우가 있으니 잘 구분할 수 있어야겠죠?

또한 그냥 table에서 시작하시면 문서 내에 table이 두 개이기 때문에 좋은 방법이 아닙니다.

