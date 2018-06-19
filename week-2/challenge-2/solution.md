# 모범 답안



```python
infos = html.select('div.cds')

print(infos[0].select('dt.title tooltip')[0].text, '/', infos[0].select('dd.chn > a')[0].text, '/', infos[0].select('span.hit')[0].text)
print(infos[1].select('dt.title tooltip')[0].text, '/', infos[1].select('dd.chn > a')[0].text, '/', infos[1].select('span.hit')[0].text)
print(infos[2].select('dt.title tooltip')[0].text, '/', infos[2].select('dd.chn > a')[0].text, '/', infos[2].select('span.hit')[0].text)
print(infos[3].select('dt.title tooltip')[0].text, '/', infos[3].select('dd.chn > a')[0].text, '/', infos[3].select('span.hit')[0].text)
print(infos[4].select('dt.title tooltip')[0].text, '/', infos[4].select('dd.chn > a')[0].text, '/', infos[4].select('span.hit')[0].text)
```

다른 순위의 클립에 접근하기 위해 클립 배열인 infos에 숫자를 바꿔서 사용하고 있습니다.

select\('dt.title tooltip'\) 등 내부 값을 select 했을 때 돌아오는 결과는 값이 하나 들어있는 배열이니 0 이외의 숫자를 넣으면 오류가 납니다.

선택자 경로가 다른 것은 괜찮습니다.  
어떤 선택자 경로가 더 의미를 알기 쉽고 짧은지 비교해보세요.

