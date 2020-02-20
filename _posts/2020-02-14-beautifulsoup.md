---
title: "BeautifulSoup활용하여 크롤링하기"
tag : 
   - Python 
   - BeautifulSoup
   - bs4
---

## BeautifulSoup이란?
 * Beautiful Soup는 HTML과 XML 파일로부터 데이터를 가져오기(크롤링) 위한 라이브러리 입니다. Beautiful Soup를 설치하기 위해 아래 명령어를 입력합니다.
 * Javascript 에 조건이 충족되어야만 얻을 수 있는 데이터에 접근하는 것에 한계가 있습니다(selenium 사용하여 해결)


 
## BeautifulSoup 사용법
 * 설치
 
```python
pip install bs4

# BeautifulSoup 모든함수 정리(https://www.crummy.com/software/BeautifulSoup/bs4/doc/#beautiful-soup-documentation)
# find("태그", {"클래스" : "클래스명"})), findAll() 차이
# find()는 찾고자하는 tag의 첫번째 항목만 가져온다. 
# findAll() 찾고자하는 tag의 모든항목을 가져온다 LIST형태로 리턴

from bs4 import BeautifulSoup
import requests

url = ''
html = requests.get(url)
soup = BeautifulSoup(html,'html.parser')

soup.select('원하는 정보')  # select('원하는 정보') -->  단 하나만 있더라도, 복수 가능한 형태로 되어있음

soup.select('태그명')
soup.select('.클래스명')
soup.select('상위태그명 > 하위태그명 > 하위태그명')
soup.select('상위태그명.클래스명 > 하위태그명.클래스명')    # 바로 아래의(자식) 태그를 선택시에는 > 기호를 사용
soup.select('상위태그명.클래스명 하~위태그명')              # 아래의(자손) 태그를 선택시에는   띄어쓰기 사용
soup.select('상위태그명 > 바로아래태그명 하~위태그명')     
soup.select('.클래스명')
soup.select('#아이디명')
soup.select('태그명.클래스명)
soup.select('#아이디명 > 태그명.클래스명)
soup.select('태그명[속성1=값1]')
```

 * 간단한 예제

```python
# Naver Main Crawling

from bs4 import BeautifulSoup
import requests

html = requests.get('https://naver.com')
bs4_obj = BeautifulSoup(html.text,'html.parser')

# 전체 페이지
print(bs4_obj)

# div, class='area_links' 추출
top_right = bsObj.find("div", {"class":"area_links"})
first_a = top_right.find("a")
print(first_a.text)
```

 * 삼성주식(종가/시가/고가/저가 가져오기)
 
```python
import requests
from bs4 import BeautifulSoup


def get_bs_obj(company_code):
    url = 'https://finance.naver.com/item/main.nhn?code=' + company_code
    html = requests.get(url)
    bs4_obj = BeautifulSoup(html.content, 'html.parser')
    return bs4_obj

def get_candle_chart_data(bs4_obj):
    rate_table = bs4_obj.find('div',{"class":"rate_info"})
    rate_tds = rate_table.findAll("td")
    #종가
    close_cost = rate_tds[0].find("span",{"class","blind"})
    #시가
    see_cost = rate_tds[3].find("span",{"class","blind"})
    #고가
    high_cost = rate_tds[1].find("span",{"class","blind"})
    #저가
    low_cost = rate_tds[4].find("span",{"class","blind"})

    return [close_cost, see_cost, high_cost, low_cost]


print(get_candle_chart_data(get_bs_obj("005930")))
```
