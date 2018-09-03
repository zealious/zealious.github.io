


## 퍼스트클래스 함수(First Class Function)란?

퍼스트클래스 함수란 프로그래밍 언어가 함수를 first-class citizen으로 취급하는 것을 뜻합니다.  
함수자체를 인자(argument)로 전달하거나 함수의 결과값으로 리턴할 수 있고 함수를 할당하거나 데이터 구조안에 저장할 수 있는 함수를 뜻합니다.

* 1급(First Class) 함수의 조건  
  * 변수에 담을 수 있다.
  * 인자로 전달할 수 있다.
  * 반환값(Return Value)으로 전달할 수 있다.
  * 런타임 생성이 가능하다.
  * 익명(Anonymous) 생성이 가능하다
 
### 변수에 담을 때
```python
def firstclass(x):
    return x
    
fc = firstclass

print(firstclass)
print(fc)

#위 2개의 결과값은 같은 참조값으로 같다.
```

### 인자로 전달 할 때
```python
# 인자로 전달할 수 있다.

def firstclass(x):
    return x * x

def calcu(func,list):
    result = []
    for i in list:
        result.append(func(i))
    return result
    
print(calcu(firstclass,list))
```

물론 위와 같이 사용하지않고 func(i)대신 i * i로 사용할 수 있지만 함수만 변경해서 재사용할 수 있는 장점이 있습니다.

```python
def firstclass1(x):
    return x * x * x
    
print(calcu(firstclass1, list))
```
### 함수를 리턴에 사용할 때
```python 
def logger(msg):
    def log_message():
        print('Log: ', msg)
    return log_messgae
    
log_hi = logger('Hi')
print(log_hi)
log_hi()

>>>
<function log_message at xxxxx>
Log: Hi
```

msg와 같은 함수의 지역변수값은 함수가 호출된 이후에 메모리상에서 사라지므로 다시 참조할 수가 없는데,  
msg 변수에 할당됐던 'Hi' 값이 logger함수가 종료된 이후에도 참조됐다는 것입니다.  
이런 log_message와 같은 함수를 "클로저(closure)"라고 부르며 다른 함수의 지역변수를 그함수가 종료된 이후에도 기억할 수 있습니다  


참조글 : 
http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%8D%BC%EC%8A%A4%ED%8A%B8%ED%81%B4%EB%9E%98%EC%8A%A4-%ED%95%A8%EC%88%98-first-class-function/
