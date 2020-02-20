---
title: "네이버 웹툰 이미지 다운 및 합치기"
tag : 
   - Python 
   - beautifulSoup
   - requests
   - pillow
---

## 네이버 웹툰 이미지 다운 및 합치기
 * 네이버 웹툰 접속하기
 * 웹툰 이미지 다운 받아보기
 * 다운받은 이미지 하나로 합치기
 
 
## 네이버 웹툰 이미지 다운받아보기
```python

from bs4 import BeautifulSoup
import requests
import os

webtoon_url = ''

html = requests.get(webtoon_url).text

bs4_obj = BeautifulSoup(html, 'html.parser')

#print(bs4_obj.select('.wt_viewer'))

for img_tag in bs4_obj.select('.wt_viewer img'):
    img_url = img_tag['src']
    data = requests.get(img_url, headers={'referer':webtoon_url}).content
    file_name = os.path.basename(img_url)
    open(file_name, 'wb').write(data)
```
