---
title: "웹스크레이핑"
tag : 
   - Python 
---

## Scraper란?
 * 도메일 이름을 받고 HTML 데이터를 가져옴
 * 데이터를 파싱해 원하는 정보를 얻음
 * 원하는정보를 저장함
 * 필요하다면 다른 페이지에서도 이 작업을 반복함
 * 필요한 모듈들(requests, **BeatuifulSoup4**, lxml)


## requests에 원하는 컨텐츠를 뽑아낸다.
```python
import requests

res = requests.get('url')

res.headers
res.encoding
res.text
res.json
```

## BeautifulSoup 소개
 * HTML(XML)을 파싱하게 좋게 파이썬 객체로 돌려준다.
 * 잘못된 HTML을 수정하여 반환해줌

# Tag 객체
```pyton
pip install beautifulsoup4

import requests
from bs4 import BeautifulSoup

res = requests.get('http://book.naver.com')
res.text

soup = BeautifulSoup(res.text, 'lxml')

# Tag를 지정해서 첫번째 div를 가져온다
div_tag = soup.div
div_tag
```

# NavigableString 객체
 * .string vs .text
   * .string : td태크모두리턴

   * .text : 모든 글자 리턴
   
## 원하는 데이터 추출하기
 * 특정 데이터를 추출하기 위해서는 해당 데이터를 포함하는 태그를 찾아야한다.

# find()와 find_all()
 * BeautifulSoup에서 가장 자주 쓰이는 함수
 * HTML 페이지에서 원하는 태그를 다양한 속성에 따라 쉽게 필터링 할 수 있음.
```python
find(tag, Attributes)     # tag.find('div')
find_all(tag, Attributes) # tag.find_all('a', href='link3')
find(Attributes), find_all(Attributes)
find(class_='box')
```
 * 정규식으로 찾기
```python
find(recompile('^b'))     # b로 시작되는 첫 번째 태그
find_all(re.compile('t')) # t를 토함한 모든 태그
find_all(href=re.ocmpile('[\w]{3}']
```

# CSS셀렉터로 찾기
 * soup.select("head > title")
 * soup.select(".sister")       # 클래스명으로 찾기
 * soup.select("a#link2")       # 아이디로 찾기
 * soup.select('a[href^="http://examlple.com/"]')  # 특정문자열로 시작
 * soup.select('a[href$="title"]')                 # 특정문자열로 
 
