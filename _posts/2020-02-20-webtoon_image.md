---
title: "네이버 웹툰 이미지 다운 및 합치기"
tag : 
   - Python 
   - beautifulSoup
   - requests
   - pillow
---

## 네이버 웹툰 이미지 다운 및 합치기
 * 네이버 웹툰 접속하기 - beautifulSoup, requests 활용
 * 웹툰 이미지 다운 받아보기 - open 함수 활용
 * 다운받은 이미지 하나로 합치기 - Pillow 활용
 
## Pillow 사용하기
```python 
# 설치하기
pip install pillow

from PIL import Image as PILImage

im = PILImage.open('test.png")  # PILImage로 image 열기
print(im.format)                # bmp, jpeg, png, gif
print(im.size)                  # (width, height) tuple형태
print(im.mode)                  # RGB, RGBA, YmCK, Gray 등 색상컬러 정보

# 이미지 열기
im.show()

# 이미지 생성하기
Image.new("RGB", (width, height), (200,200,200)) # 모드, 사이즈, 바탕색(default 검은색)

# 이미지 자르기
new_img = im.croup((100, 100, 500, 500))  # 가로시작점, 세로시작점, 가로범위, 세로범위

# 이미지 붙이기
im.paste(new_img, (0, 0))

# 사이즈변경
im.resize((100,100)) # tuple형태의 인자값

# 회전하기
im.rotate(45)   # 45도 회전

# Filter 효과
img.filter(ImageFilter.BLUR)  # 아래효과 중 선택
"""
BLUR
CONTOUR
DETAIL
EDGE_ENHANCE
EDGE_ENHANCE_MORE
EMBOSS
FIND_EDGES
SMOOTH
SMOOTH_MORE
SHARPEN
"""
```
 
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

## Pillow를 사용하여 이미지 (일부)붙이기
```python
# canvas 최대 높이의 문제로이미지 파일 전체가 들어가지않고 일부만 들어감
from bs4 import BeautifulSoup
import os
import requests
from PIL import Image as PILImage

wt_url = 'https://comic.naver.com/webtoon/detail.nhn?titleId=183559&no=469&weekday=mon'


wt_html = requests.get(wt_url).text
bs4_obj = BeautifulSoup(wt_html,'html.parser')

# print(bs4_obj.select('.wt_viewer img'))
img_name_list = []

for img_tag in bs4_obj.select('.wt_viewer img'):
    img_url = img_tag['src']
    img_data = requests.get(img_url, headers = {'referer':wt_url}).content
    file_name = os.path.basename(img_url)
    with open(file_name,'wb') as img_save:
        img_save.write(img_data)
        
    img_name_list.append(file_name)


# open_img = []
# for filename in img_name_list:
#     open_img.append(PILImage.open(filename))
# List Comprehension 사용하기    
open_img = [PILImage.open(filename) for filename in img_name_list]

# Canvas 사이즈 정하기 
# 세로사이즈가 65000이 넘으면 안되게 min함수 사용
canvas_size = [
        max([i.width for i in open_img]),#가로
        min(65000, sum([i.height for i in open_img]))#세로
]


canvas = PILImage.new('RGB', canvas_size, (255, 255, 255))

top = 0
for im in open_img:
    canvas.paste(im, (0, top))
    top += im.size[1]

# 전체 이미지파일이 65000을 넘으므로 일부만 들어가있음
canvas.save('신의탑(일부).jpg')
```

## Pillow를 사용하여 이미지 (전체)붙이기
```python
# 이미지 높이 사이즈 조정 후 이미지 파일 전체 들어가게 수정

from bs4 import BeautifulSoup
import os
import requests
from PIL import Image as PILImage

wt_url = 'https://comic.naver.com/webtoon/detail.nhn?titleId=183559&no=469&weekday=mon'


wt_html = requests.get(wt_url).text
bs4_obj = BeautifulSoup(wt_html,'html.parser')

# print(bs4_obj.select('.wt_viewer img'))
img_name_list = []

for img_tag in bs4_obj.select('.wt_viewer img'):
    img_url = img_tag['src']
    img_data = requests.get(img_url, headers = {'referer':wt_url}).content
    file_name = os.path.basename(img_url)
    with open(file_name,'wb') as img_save:
        img_save.write(img_data)
        
    img_name_list.append(file_name)


# 65000사이즈를 넘지않게 리사이징하는 과정 
# 65000을 넘지않을경우 기존 파일 그대로 리턴
def height_max_check(img_name_list, sum_height):
    # open_img = []
    # for filename in img_name_list:
    #     open_img.append(PILImage.open(filename))
    open_img = [PILImage.open(filename) for filename in img_name_list]
    sum_height = sum([i.height for i in open_img])
    
    if sum_height > 65000:
        re_height = 65000 / len(open_img)
        small_file_nm = []
        for i in open_img:
            small_obj = i.resize((i.width, int(re_height)))
            rename = 'resize_' + i.filename
            small_obj.save(rename)
            small_file_nm.append(rename)
        return [PILImage.open(small_file) for small_file in small_file_nm]
    return open_img


open_img = height_max_check(img_name_list, sum_height)

# print(open_img[0].size)
# print(open_img[0].width)
# print(open_img[0].height)


canvas_size = [
        max([i.width for i in open_img]),#가로
        min(65000, sum([i.height for i in open_img]))#세로
]

top = 0
canvas = PILImage.new('RGB', canvas_size, (255, 255, 255))

for im in open_img:
    canvas.paste(im, (0, top))
    top += im.size[1]

canvas.save('신의탑(전체).jpg')
```
