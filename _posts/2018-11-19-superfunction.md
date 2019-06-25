---
title: "super로 부모 클래스를 초기화하자"
tag : Python
---

기존에는 자식 클래스에서 부모 클래스의 __init__메서드를 직접 호출하는 방법으로 부모 클래스를 초기화했다.  
```python
class MyBaseClass():
    def __init__(self, value):
        self.value = value

class MyChildClass(MyBaseClass):
    def __init__(self):
        MyBaseClass.__init__(self, 5)
```

이 방법은 간단한 계층 구조에는 잘동작하지만 많은 경우 제대로 동작하지 못한다.
다이아몬드 상속은 서브클래스가 계층 구조에서 같은 슈퍼클래스를 둔 서로 다른 두 클래스에서 상속받을 때 발생한다.
```python
class TimeFive(MyBaseClass):
    def __init__(self, value):
        MyBaseClass.__init__(self, value)
        self.value *= 5

class PlusTwo(MyBaseClass):
    def __init__(self, value):
        MyBaseClass.__init__(self, value)
        self.value +=2

class ThisWay(TimesFive, PlusTwo):
    def __init__(self, value):
        TimesFive.__init__(self, value)
        PlusTwo.__init__(self, value)

foo = ThisWay(5)
print('Should be (5 * 5) + 2 = 27 but is', foo.value)

>>>
Should be (5 * 5) + 2 = 27 but is 7
```

결과가 27이어야 하지만 두번째 부모클래스의 생성자 PlusTwo.__init__를 호출하는 코드가 있어서  
MyBaseClass.__init__가 두번 째 호출될 때 self.value를 다시 5로 리셋한다.

이 문제를 해결하려고 super라는 내장함수를 추가하고 메서드 해석순서(MRO,Method Resolution Order)를 정의했다.  
MRO는 어떤 슈퍼클래스부터 초기화하는지를 정한다.(예를들면 깊이우선, 왼쪽에서 오른쪽으로)
또한 이 다이아몬드 계층 구조에 있는 공통 슈퍼클래스를 단 한 번만 실행하게 한다.
```python
class TimesFiveCorrect(MyBaseClass):
    def __init__(self, value):
        super(TimesFiveCorrect, self).__init__(value)
        self.value *= 5
        

class PlusTwoCorrect(MyBaseClass):
    def __init__(self, value):
        super(PlusTwoCorrect, self).__init__(value)
        self.value += 2

class GoodWay(TimesFiveCorrect, PlusTwoCorrect):
    def __init__(self, value):
        super(GoodWay, self).__init__(value)

foo = GoodWay(5)
print('Should be 5 * (5 + 2) = 35 and is', foo.value)

from pprint import pprint
print(GoodWay.mro())

>>> 
Should be 5 * (5 + 2) = 35 and is 35

mro출력순서 -> GoodWay -> TimesFiveCorrect -> PlusTwoCorrect -> MyBaseClass -> object
```

생성자의 호출은 mro 출력순서의 역순으로 실행된다.
super는 제대로 동작하지만, 주목해야할 2가지문제가있다.

1) 문법이 장황하여 파이썬을 처음 접하는 프로그래머에게 혼란을 줄 수 있다.  
2) suepr를 호출하면서 현재 클래스의 이름을 지정해야 한다.  
   클래스의 이름을 변경(클래스 계층 구조를 개선할 때 아주 흔히 하는 조치다)하면 super를 호출하는 모든 코드를 수정해야한다.
   
```python
class TimesFiveCorrect(MyBaseClass):
    def __init__(self, value):
        super(__class__, self).__init__(value)
        
class PlusTwoCorrect(MyBaseClass):
    def __init__(self, value):
        super(__class__, self).__init__(value)
```

파이썬 3에서는 __class__변수를 사용한 메서드에서 현재 클래스를 올바르게 참조하도록 해주므로 위의 코드가  잘 동작한다.

> 파이썬의 표준 메서드 해석순서(MRO)는 슈퍼클래스의 초기화 순서와 다이아몬드 상속 문제를 해결한다.  
> 항상 내장 함수 super로 부모 클래스를 초기화하자.
