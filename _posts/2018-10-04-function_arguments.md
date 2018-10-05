---
title: "함수의 인수(가변인수, 키워드인수)"
tag : Python
---


## 가변 위치 인수로 깔끔하게 보이게 하자

선택적인 위치인수(*args 'star args'라고 한다)를 받게 만들면 함수 호출을 더 명확하게 할 수 있고  
보기에 방해가 되는 요소를 없앨 수 있다.

```python
# 로그를 남기는 함수
def log(message, values):
    if not values:
        print(message)
    else:
        values_str = ', '.join(str(x) for x in valus)
        print('%s: %s' % (message, values_str))

log('My numbers are', [1, 2])
log('Hi there', [])

>>
My numbers are: 1, 2
Hi there
```

위와 같이 로그를 남길 값이 없을 때 빈 리스트를 넘겨야 한다는 건 불편한일이다.  
두번 째 인수를 받지 않을 수 있으면 더 좋을 것이다.


```python
# 로그를 남기는 함수
def log(message, *values):  # 유일하게 다른부분
...위와 동일

log('My numbers are', [1, 2])
log('Hi there')  # 사용하기 좋아졌다.

favorites = [7, 33, 99]
log('Favorite colors', *favorites)

>>
My numbers are: 1, 2
Hi there

Favorite colors: 7, 33, 99
```

리스트를 log같은 가변인수 함수를 호출하는데 사용하고 싶으면 * 연산자를 쓰면된다.(위 favorites 참고)

가변 개수의 위치 인수를 받는 방법에는 2가지 문제가 있다.

> 1) 가변 인수가 함수에 전달되기에 앞서 항상 튜플로 변환된다.  
> 제너레티어를 사용할 경우 * 연산자를 쓰면 제너레이터가 모두 소진될 때까지 순환됨을 의미한다.(메모리문제 발생)  
> *args인수를 사용할 때에는 입력인수가 적당히 적다는 사실을 알 때 가장 좋은 방법이다.  
> 2) 코드를 모두 변경하지 않고서는 새 위치 인수를 추가할 수 없다는 점이다.  
>   
> 인수 리스트의 앞쪽에 위치 인수를 추가하면 기존의 호출 코드가 수정 없이는 이상하게 동작한다.

```python
def log(sequence,message, *values):
...

log(1, 'Favorite', 7, 33) # 잘 동작한다
log('Favorite', 7, 33)    # 수정하지 않으면 이상하게 동작한다.

>>>
1: Favorite: 7, 33
Favorite numbers: 7: 33 # 잘못됨
```
이런 문제를 해결하기 위해서는 **keyword arguments** 를 사용하자.

## 키워드 인수로 선택적인 동작을 제공하자.

```python
def log(sequence,message):
...

log(message=2, 1) # error 발생 위치인수는 키워드 인수 앞에 지정해야한다.
log(2, sequence=1) # error 발생 각 인수는 한번만 지정이가능(sequence가 2번 지정됨)
```

