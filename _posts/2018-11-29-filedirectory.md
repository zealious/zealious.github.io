---
title: "파일과 디렉토리 다루기"
tag : 
   - Python 
---

## pickle 모듈
```python
# 리스트나 클래스를 파일에 저장할 때 사용
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
``
