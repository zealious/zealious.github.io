---
title: "헬퍼 함수"
tag : Python
---


## 헬퍼함수란?

자주사용해야하는 로직이지만 읽기 어렵고 복잡해보이는 경우  
코드를 짧고 명확하게 이해할 수 있게 만들어 주는게 헬퍼함수 입니다.


## 복잡한 표현식 대신 헬퍼 함수를 작성하자

* 무조건 짧은 코드를 만들기 보다는 가독성을 선택하는 편이 낫다.

```python
you_dict = { 'A' : 'red', 'B' : '1', '' }

print(you_dict.get('A'))            # 있는그대로 출력
print(you_dict.get('blue') or 0)    # None 이거나 '' 비어있는값일 경우 0으로 출력
print(int(you_dict.get('B') or 0))  # 값을 계산하고싶을 경우 int로 치환해야한다. 

#세번째 출력은 복잡해서 아래와 같이 표현 할 수 있다.
b = you_dict.get('B')
b = int(b[0]) if b[0] else 0        # if 문을 통해 더 명확하게 표현할 수 있다.

# 위 로직을 펼쳐보자
b = you_dict.get('B')
if b[0]:
    b = int(b[0])
else:
    b = 0

# 이 로직을 반복해서 자주 사용한다면 헬퍼함수를 만든다.
def you_help_function(values, key, default=0):
    found = values.get(key)
    if found[0]:
        found = int(found[0])
    else:
        found = default
    return found
    
b = you_help_function(you_dict, 'B')
```

## 핵심정리

* 파이썬 문법을 이용해 한 줄짜리 표현식을 쉽게 작성할 수 있지만 코드가 복잡하고 읽기 어려워진다.
* 복잡한 표현식은 헬퍼 함수로 옮기는게 좋다 특히 같은 로직을 반복해서 사용해야하는 경우
* if/else 표현식을 이용하면 or나 and 같은 부울 연산자를 사용할 때보다 읽기 수월한 코드를 작성할 수 있다.
