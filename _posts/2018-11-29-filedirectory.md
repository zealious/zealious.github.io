---
title: "파일과 디렉토리 다루기"
tag : 
   - Python
   - pickle
   - shutil
   - directory
   - zipfile
---

## pickle 모듈
```python
# LIST, Dict, Class를 파일에 저장할 때 사용
# pickle 모듈을 사용할 때 주의할 점은 데이터를 저장, 불러올 때에 파일 형식을 바이트(b) 형식으로 읽고 써야 한다는 점이다.
# pickle의 장점 https://korbillgates.tistory.com/173
# 바이너리와 텍스트 차이 이해 
# https://m.blog.naver.com/PostView.nhn?blogId=tipsware&logNo=221353023593&proxyReferer=https%3A%2F%2Fwww.google.com%2F
import pickle

colors = ['red', 'green', 'black']

f = opne('colors.pickle', 'wb')
pickle.dump(colors, f)
f.close()

del colors

f = open('colors.pickle', 'rb')
colors = pickle.load(f)
print(colors)
f.colse()
```

## 파일과 디렉터리 관리

> 하드디스크에 저장된 파일과 디렉터리의 관리 작업을 자동화  
> 특정 확장자를 가진 파일 또는 특정 이름 패턴을 가진 파일을 지우거나 다른 디렉터리로 옮기기  
> 파일이나 디렉터리를 압축하여 별도 저장(일종의 백업 시스템)

### shutil(shell utils) 모듈
* 파일이나 디렉터리를 복사, 이동, 리네임, 삭제를 수행하는 모듈

```python
# copy
import shutil, os
os.chdir('C:\\')
shutil.copy('C:\\spam.txt', C:\\delicious')
>>>
'C:\\delicious\\spam.txt'    # 복사된 이름이 리턴
shutil.copy('C:\\egg.txt', 'C:\\delicious\\eggs2.txt')
>>>
'C:\\delicious\\eggs2.txt'   # 복사된 이름이 리턴

# move
# 옮기는 위치에 파일이있으면 이동 불가
shutil.move('C:\\spam.txt', 'C:\\delicious')
>>
'C:\\delicious\\spam.txt'
shutil.move('C:\\spam.txt', 'C:\\delicious\\new_spam.txt')
>>>
'C:\\delicious\\new_spam.txt'

# 삭제
os.unlink(path) / os.remove(path)
os.rmdir(path)      # 해당 경로가 비어있어야함.
shutil.rmtree(path) # 경로가 비어있지 않아도 모든 파일이 삭제됨
```

## 안전 삭제(send2trash 모듈)
```python
pip install send2trash

import send2trash
baconFile = open('bacon.txt', 'a')  # creates the file
baconfFile.write('Bacon is not a vegetable.')

baconFile.close()
send2trash.send2trash('bcon.txt')
```

## 디렉토리 트리 운행

```python
import os

for folder_name, subfolders, filenames in os.walk('C:\\delicious'):
    print('The current folder is ' + folder_name)
    
    for subfolder in subfolders:
        print('SUBFOLDER OF ' + folder_name + ' : ' + subfolder)
    for filename in filenames:
        print('FILE INSIDE ' + folder_name + ' : ' + filename)
        
    print('')
```

## zipfile 모듈을 이용하여 파일 압축과 풀기
```python

# 압축 된 내용보기
import zipfile, os

exampleZip = zipfile.ZipFile('example.zip')   # zip파일의 객체 생성
exampleZip.namelist()                         # 압축된 내용 리스트로 리턴
>>> ['spam.txt', 'cats/']
spamInfo = exampleZip.getinfo('spam.txt')
spamInfo.file_size
spamInfo.compress_size
exampleZip.close()

# 압축하기 
# 'w' 모드를 사용하여 새로운파일을 만들고 기존에 있으면 내용이 지워진다.
# 'a' 컨텐츠 추가 모드
newZip = zipfile.ZipFile('new.zip', 'w')
newZip.write('spam.txt', compress_type=zipfile.ZIP_DEFLATED)
newZip.close()

#압축 풀기
exampleZip.extractall()
exampleZip.close()

```
