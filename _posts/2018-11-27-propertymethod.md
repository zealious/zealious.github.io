---
title: "게터와 세터 메서드 대신에 일반 속성을 사용하자"
tag : Python
---
 
 
 다른 언어에서 파이썬을 ㅗ넘어온 프로그래머들은 자연스레 클래스에 게터와 세터 메서드를 명시적으로 구현하려고한다.
 ```python
 class OldResistor():
     def __init__(self, ohms):
         self._ohms = ohms
         
     def get_ohms(self):
         return self._ohms
     
     def set_ohms(self, ohms):
         self._ohms = ohms

```
 
 이런 게터와 세터를 사용하는 방법은 간단하지만 파이썬답지 않다.
 ```python
 r0 = OldResistor(50e3)
 
 r0.set_ohms(r0.get_ohms() + 5e3)
 ```
 이런 유틸리티 메서드는 클래스의 인터페이스를 정의하는 데 도움이 되고,  
 기능을 캡슐화하고 사용법을 검증하고 경계를 정의하기 쉽게 해준다.  
 이런 요소는 클래스가 시간이 지나면서 발전하더라도 호출하는 쪽 코드를 절대 망가뜨리지 않도록 설계할 때 중요한 목표가 된다.
 
 하지만 파이썬에서는 명시적인 게터와 세터를 구현할 일이 거의 없다.  
 대신 항상 간다한 공개속성부터 구현하기 시작해야 한다.
 
 ```python
 class Resistor():
     def __init__(self, ohms):
         self.ohms = ohms
         self.voltage = 0
         self.current = 0
 r1 = Resistor(50e3)
 r.ohms = 10e3
 ```
 
 이렇게 하면 즉석에서 증가시키기 같은 연산이 자연스럽고 명확해진다.
 r1.ohms += 5e3
 
 나중에 속성을 설정할 때 특별한 동작이 일어나야 하면 @property 데코레이터(decorator)와 이에 대응하는  
 setter 속성을 사용하는 방법으로 바꿀 수 있다. 여기서는 Resistor의 새 서브클래스를 정의하여 voltage프로퍼티(property)를  
 할당하면 current값이 바뀌게 해본다.  제대로 동작하려면 세터와 게터 메서드의 이름이 의도한 프로퍼티 이름과 일치해야 한다.
 
 ```python
 class VoltageResistance(Resistor):
     def __init__(self, ohms):
         super().__init__(ohms)
         self._voltage = 0
     
     @property
     def voltage(self):
         return self._voltage
     
     @voltage.setter
     def voltage(self, voltage):
         self._voltage = voltage
         self.current = self._voltage / self.ohms
```

부모 클래스의 속성을 불변으로 만드는 데도 @property를 사용할 수 있고  
특정조건을 걸어서 에러를 발생시킬 수 도 있다.

```python
@ohms.setter
def ohms(self, ohms):
    if ohms <= 0:
        raise VlaueError('ValueError')
    self._ohms = ohms
    
@ohms.setter
def ohms(self, ohms):
    if hasattr(self, '_ohms'):
        raise AttributeError('Can't set attribute')
    self._ohms = ohms
```

@property의 가장 큰 단점은 속성에 대응하는 메서드를 서브클래스에서만 공유할 수 있다는 점이다.
이 단점은 디스크립터(descriptor)를 통해 보완할 수 있다.  

마지막으로 @property 메서드로 세터와 게터를 구현할 때 예상과 다르게 동작하지 않게 해야 한다.  
예를들면 게터 프로퍼티메서드에서는 다른 속성을 설정하지 말아야 한다.
  
최선의 정책은 @property.setter 메서드에서만 관련 객체의 상태를 수정하는 것이다.
  
사용자는 다른 파이썬 객체가 그렇듯이 클래스의 속성이 빠르고 쉬울 거라고 기대할 것이다.
더 복잡하거나 느린 작업은 일반 메서드로 하자.


> 간단한 공개 속성을 사용하여 새 클래스 인터페이스를 정의하고 세터와 게터 메서드는 사용하지말자  
> 객체의 속성에 접근할 때 특별한 동작을 정의하려면 @property를 사용하자  
> @property 메서드에서 최소 놀람 규칙을 따르고 이상항 부작용은 피하자  
> @property메서드가 빠르게 동작하도록 만들자. 느리거나 복잡한 작업은 일반 메서드로 하자.

