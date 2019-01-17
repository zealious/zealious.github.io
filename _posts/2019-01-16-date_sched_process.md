---
title: "날짜, 스케쥴링, 프로세스 "
tag : 
   - Python 
---


## time 모듈
 * 날짜와 시간을 표현하는 형식은 너무도 다양함
 * time 모듈 : 절대시간(epoch 타임)을 다루는 모듈
 * datetime 모듈 : 시간을 보기 좋게 표현하는 방식에 대한 모듈
 
### time.time() 함수
 * 1970.01.01. 00:00:00 -> 에포크 타임스탬프
 * 에포크로 부터의 경과시간을 숫자(float 타입)로 표시
 * rount(time.time())으로 소수점을 없애서 사용가능
 
 * now = time.time()
 * time.ctime(now)
 * time.localtime(now) - 현지시간(컴퓨터시간)
 * time.gmtime(now)    - 그리니치 mean time
 
### 시간기리 비교가능 - 즉 미래가 과거보다 크다


### time.sleep() 함수
 * 프로그램을 잠시 중지할 필요가 있을 때 사용
 * time.slee(second) : 해당 second(초) 만큼 프로그램이 일시 중지
 * KeyboardInterrupt 예외를 처리해서 Ctrl + C 키를 눌렀을 때 종료 가능
```python
for i in range(30):
    time.sleep(1)
    print(i)
```

## datetime 모듈
 * date : 년, 월, 일
 * time : 시, 분, 초, 마이크로초(하루의 시간을 나타내는 데 사용핟나.)
 * datetime : 날짜와 시간  
   > format : datetime.dateime(year, month, day, hour, minute, second, micro)  
   > isoformat() 메소드 사용가능
   > now()로 현재 날짜를 얻을 수 있다.
 * timedelta : 날짜 와/또는 시간 간격(datetime.now + timedelta(days=1000))
```python
from dateime import date, time
white = date(2017, 3, 14)
white
>>> datetime.date(2017, 3, 14)
white.month
>>> 3
white.day
>>> 14
white.year
>>> 2017
white.isoformat()
>>> '2017-03-14'

bedtime = time(22, 35, 45)
bedtime
>>> datetime.time(22, 35, 45)
bedtime.hour; bedtime.minute; bedtime.second
```

## datetime 객체를 문자로 바꾸기
 * strftime() 함수로 보기 좋은 포맷으로 변경
```python
wc8 = datetime.datetime(2002, 6, 22, 15, 30, 0)
wc8.strftime('%Y/%m/%d %H:%M:%S')
>>> '2002/06/22 15:30:00'
wc8.strftime('%I:%M %p')
>>> '03:30 PM'
wc8.strftime("%B of '%y")
>>> "June of '02"
```


## 다른 프로그램 실행
  ### subprocess 모듈사용(어플리케이션 실행)
  ```python
  import subprocess
  subprocess.Popen('C:\\Window\\Syste32\\calc.exe')
  
  #OS 가 우분투일때
 subprocess.Popen('/usr/bin/gnome-calculator')
 
 subprocess.Popen(['C:\\python34\\python.exe, 'hello.py'])
 #기본 프로그램으로 파일 열기
 subprocess.Popen(['start', 'hello.txt'], shell=True)
 
 subprocess.Poen(['python.exe', 'hello.py'])
  ```
