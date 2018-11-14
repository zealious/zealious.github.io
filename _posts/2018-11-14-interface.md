---
title: "인터페이스가 간단하면 클래스 대신 함수를 받자."
tag : Python
---


다른 언어에서라면 후크를 추상 클래스로 정의할 것이라고 예상할 수도 있다.  
하지만 파이썬의 후크 중 상당수는 인수와 반환값을 잘 정의해놓은 단순히 상태가 없는 함수다
  
함수는 클래스보다 설명하기 쉽고 정의하기도 간단해서 후크로 쓰기에 이상적이다.  
함수가 후크로 동작하는 이유는 파이썬이 일급함수를 갖췄기 때문이다.

```python
def log_missing():
    print('key added')
    return 0

current = {'green' : 12, 'blue' : 3}
increments = [
    ('red', 5),
    ('blue', 17),
    ('orange', 9),
]

result = defaultdict(log_missing, current)
print('Before:', dict(result))
for key, amount in incerements:
    result[key] += amount
print('After: ', dict(result))

>>>
Before: {'green' : 12, 'blue' : 3}
Key added
Key added
After: {'orange' : 9, 'green' : 12, 'blue' : 20, 'red' : 5}
```

보통의 경우 함수를 사용하지만 상태 보존 클로저를 사용해야하는 경우 클래스를 사용하면 더 좋다.  
파이썬에서 __call__ 메서드는 객체르 함수처럼 호출할 수 있게해준다.
```python
class BetterCountMissing():
    def __init__(self):
        self.added = 0
    def __call__(self):
        self.added += 1
        return 0
        
counter = BertterCountMissing()
counter()
assert callable(count)

result = defaultdict(count, current)
for key, amount in incremetns:
    result[key] += amount
assert counter.added == 2
```

> 파이썬에서 컴포넌트 사이의 간단한 인터페이스용으로 클래스를 정의하고 인스턴스를 생성하는 대신에 함수만 써도 종종 충분하다
> __call__이라는 특별한 메서드는 클래스의 인서턴스를 일반 파이썬 함수처럼 호출할 수 있게 해준다.
> 상태보존하는 함수가 필요할 때 클로저를 정의하는 대신__call__메서드를 제공하는 클래스를 정의하는 방안을 고려하자
