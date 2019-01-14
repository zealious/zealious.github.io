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
