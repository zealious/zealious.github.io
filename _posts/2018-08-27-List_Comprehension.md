---
title: "map과 filter대신 리스트 컴프리헨션을 사용하자"
tag : Python
---

## lambda함수
익명함수라고 불리며 function을 한줄로 작성 할 수 있는 방법이다.
간단한 함수 같은 경우 lambda를 이용해서 사용한다.

```python
# lambda 인자: 표현식

f = lambda x:x+1
print("f(1): ", f(1))
```


## map과 filter함수

* map은 동일한 함수에 리스트의 값을 인자로보내서 리턴값을 받을 때 사용한다.

```python
# map(function, list)

map_value = map(lambda x: x**2 , a)
```

* filter는 리스트의 값을 함수조건에 해당할 경우 남겨두고 나머지는 걸러낸다.

```python
# filter(function, list)

filter_value = filter(lambda x: x %2 == 0 , list)
```

## 리스트 컴프리헨션(list comprehension: 리스트 함축 표현식)

