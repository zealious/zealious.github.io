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

* map과 filter를 사용할 수 있지만 리스트 컴프리헨션을 사용하면 보기가 훨씬 좋다.

```python
# list comprehension
compre = [x**2 for x in list if x % 2 == 0]

# map & filter
map_filter = map(lambda x: x**2, filter(lambda x: x%2 == 0, list))

assert compre == list(map_filter) # assert 해당 조건이 맞지않을경우 AssertionError 발생
```

```python
chile_ranks = {'ghost': 1, 'habanero': 2, 'cayenne': 3}

# Dictionary
rank_dict = {rank, name for name, rank in chile_ranks.items()}

# Set 
chile_len_set = {len(name) for name in rank_dict.value()}

print(rank_dict)
print(chile_len_set)

>>>
{1: 'ghost', 2: 'habanero', 3: 'cayenne'}
{8, 5, 7}
```


## 리스트 컴프리헨션에서 표현식을 두 개 넘게 쓰지 말자

* 2개의 표현식은 이해하기 괜찮지만 3개 이상부터는 읽기가 쉽지않다.
* 몇 줄을 절약한 장점이 나중에 겪을 어려움보다 크지는 않다.

```python
# 2개의 표현식
squared = [[x**2 for x in row] for row in matrix]

# 3개의 표현식
flat  = [x for sublist1 in my_lists
         for sublist2 in  sublist1
         for x in sublist2]

# 3개의 표현식은 아래와 같이 표현하는게 보기 쉽다.
flat = []
for sublist1 in my_lists:
    for sublist2 in sublist1:
         flat.append(sublist2)
```

* 다중 if 조건을 지원하는데 여러 조건이 있으면 암시적인 and 표현식이 된다.

```python
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
b = [x for x in a if x > 4 if x % 2 == 0]
c = [x for x in a if x > 4 and x % 2 == 0]
```


## 입력이 클때는 제너레이터 표현식을 고려하자

* 제너레이터 표현식은 실행될 때 출력 시퀀스를 모두 구체화(메모리에 로딩)하지 않는다.
* 표현식에서 한 번에 한 아이템을 내주는 이터레이터로 평가된다.
* 제너레이터표현식은 ()문자사이에 리스트 컴프리헨션과 비슷한 문법을 사용하여 생성한다.


```python
it (len(x) for x in open('/tmp/my_file.txt'))
print(it)

>>>
# 제너레이터 표현식은 즉시 이터레이터(iterator)로 평가되므로 더는 진행되지 않는다.
<generator object <genexpr> at xxxxxx>

print(next(it))  # 내장함수 next로 한 번에 전진 시킨다.
```
## 핵심정리
* 리스트 컴프리헨션은 다중루프와 루프레벨별 다중 조건을 지원한다.
* 표현식이 두개가 넘에 들어 있는 리스트 컴프리헨션은 이해하기 매우 어려우므로 피해야한다.
* 제너레이터 표현식은 이터레이터로 한번에 한출력만 만드므로 메모리 문제를 피할 수 있다.
* 제너레이터 표현식은 서로 연결되어 있을때 매우 빠르게 실행된다.
