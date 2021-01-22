---
title: "파일 다루기"
tag : 
   - Python 
   - file
   - path
   - directory
   - open
   - with
   - sys.argv
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

```python
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

# with 와 함께쓰면 close할 필요가 없다.
with open('file.txt', 'w') as f:
    f.write('test')
```


```python
# 명령 프롬프트창에서 매개변수를 직접 주어 프로그램을 실행하는방식
# 명령 프롬프트 명령어 [인수1 인수2 ...]

#sys1.py
import sys

args = sys.argv[1:]
for i in args:
    print(i)
    
# python sys1.py   aaa     bbb 
         argv[0] argv[1]  argv[2]

```
