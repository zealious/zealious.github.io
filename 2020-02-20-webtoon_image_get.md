---
title: "네이버 웹툰 크롤링해보기"
tag : 
   - Python 
   - beautifulSoup
   - Pillow
   - requests
---

## 네이버 웹툰 이미지 다운로드 및 하나로 합치기
 * 네이버 웹툰에 접속
 * 각각의 이미지 다운로드 해보기
 * Pillow를 활용하여 하나로 붙여보기
 
## 네이버 웹툰 이미지 다운로드
 
```python
from bs4 import BeautifulSoup
import requests
import os

webtoon_url = '웹툰 URL'

html = requests.get(webtoon_url).text

bs4_obj = BeautifulSoup(html, 'html.parser')

#print(bs4_obj.select('.wt_viewer'))

for img_tag in bs4_obj.select('.wt_viewer img'):
    img_url = img_tag['src']
    data = requests.get(img_url, headers={'referer':webtoon_url}).content
    file_name = os.path.basename(img_url)
    open(file_name, 'wb').write(data)
        
```
