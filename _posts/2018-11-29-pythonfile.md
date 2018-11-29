---
title: "게터와 세터 메서드 대신에 일반 속성을 사용하자"
tag : 
   - Python 
---

## 파일과 파일경로
```python
import os
os.path.join('usr','bin','spam')

>>>
/usr/bin/spam

myFiles = ['accounts.txt', 'details.csv']
for filename in myFiles:
    print(os.path.join('C:\\User\\python', filemname))
>>>
C:\\User\\python\\accounts.txt
C:\\User\\python\\details.csv
```

## 윈도우의 dir과 리눅스의 ls명령과 유사한 glob모듈
```
# 현재 디렉터리의 모든파일을 리스트로 반환
import glob

glob.glob('*')
>>>
['index.html', ...]

glob.glob('*.py')
>>>
['test.py',...]

for filename in glob.glob('*.py'):
    print(filename)
```

## 현재 작업 디렉터리 : Current Working Directory
```python
import os
os.getcwd() # 현재 작업디렉터리를 보여줌
os.chdir('C:\\Window\\System32') # 디렉터리 변경

os.getcwd()
>>> 
C:\\Window\\System32
```

## Dir name / base name
```python
import os
path = 'C:\\Window\\calc.exe'
os.path.basename(path)
os.path.dirname(path)
>>>
calc.exe
C:\\Window
```

## 파일 쓰기와 읽기
```python
sales = open('spam_orders.txt', 'w')
sales.write('The Spam')
sales.write('Sales')
sales.close()

dollar = open('dollar.txt', 'r')
print(dollar.read())
dollar_spam.close()
```

### readline / readlines 함수 사용
2가지의 차이점
> readline 사용을 권장  
> readlines는 데이터가 클 경우 메모리 문제 발생(파일이 작을때 주로 사용)
```
# readline - 한 라인식
f = open('file.txt', 'r')
while True:
    line = f.readline()
    if not line: break
    print(line)
    
# readlines - 여러 라인을 list에 담는다
f = open('file.txt', 'r')
lines = f.readlines()

for line in lines:
    print(line)
f.close()

with open('file.txt', 'w') as f:
    f.write('test')
```

