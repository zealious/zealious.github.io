---
title: "공개 속성보다는 비공개 속성을 사용하자"
tag : Python
---
```
파이썬에는 클래스 속성의 가시성(visibility)이 공개(public)와 비공개(private)두 유형밖에 없다.  
```
```python
class MyObject(object):
    def __init__(self):
        self.public_field = 5
        self.__private_field = 10
        
    def get_private_field(self):
        return self.__private_field

foo =MyObject()

# 공개 속성은 어디서든 객체에 점 연산자(.)를 사용하여 접근할 수 있다.
assert foo.public_field == 5

# 같은 클래스에 속한 메서드에서는 비공개 필드에 직접 접근할 수 있다.
assert foo.get_private_field() == 10

# 하지만 클래스외부에서 직접 비공개 필드에 접근하면 예외가발생한다.
foo.__private_field

>>> Attribute Error: 
```

비공개 필드라는 용어에서 예상할 수 있듯이 서브클래스에서는 부모 클래스의 비공개 필드에 접근할 수 없다.

```python
class MyParentObject(object):
    def __init__(self):
        self.__private_field = 71

class MyChildObject(MyParentObject):
    def get_private_field(self):
        return self.__private_field
        
baz = MyChildObject()
baz.get_private_field()

>>>
AttributeError: 'MyChildObject' object has no attribute

assert baz._MyParentObject__private_field == 71
```

비공개 속성의 동작은 간단하게 속성 이름을 변환하는 방식으로 구현된다.  
파이썬 컴파일러는 MyChildObject.get_private_field 같은 메서드에서 비공개 속성에 접근하는 코드를 발견하면  
__private_field를 _MyChildObject__private_field에 접근하는 코드로 변환한다.  

비공개 속성용 문법이 가시성을 엄격하게 강제하지 않는 이유가 뭘까??  
파이썬 프로그래머들은 개방으로 얻는 장점이 폐쇄로 얻는 단점보다 크다고 믿는다.

```python
class MyClass(object):
   def __init__(self, value):
       self.__value = value
   
   def get_value(self):
       return str(self.__value)
       
class MyIntegerSubclass(MyClass):
    def get_value(self):
        return int(self._MyClass__value)
       
```

이럴경우 기존메서드의 결함을 해결 하기위해 서브클래스를 만들기 마련이다.  
비공개 속성을 선택하면 서브클래스의 오버라이드와 확장을 다루기 어렵고 불안정하게 만들 뿐이다.  
나중에 맏늘 서브클래스에서 꼭 필요하면 접근할 수 있긴(self._MyClass__value) 하지만 클래스 계층이 변경되면  
비공개 참조가더는 유효하지 않게 되어 제대로 동작하지 않는다.

```python
class MyBaseClass(object):
   def __init__(self, value):
       self.__value = value
   
class MyClass(object):
    # ...
    
class MyIntegerSubclass(MyClass):
    def get_value(self):
        return int(self._MyClass__value)
```

이제 __value 속성을 MyBaseClass에서 할당하므로 self._MyClass_value가 동작하지 않는다.  

일반적으로 보호속성을 사용해서 더 많은 일을 할 수 있게 하는 편이 낫다.  
각 보호필드를 문서화해서 서브클래스에서 내부 API중 어느 것을 쓸 수 있고 어느 것을 그대로 둬야 하는지 설명하자.  
```python
class MyBaseClass(object):
   def __init__(self, value):
       # 사용자가 객체에 전달한 값을 저장한다.
       # 객체에 할당하고 나면 불변으로 취급해야 한다.
       self.__value = value
```

비공개 속성을 사용할지 진지하게 고민할 시점은 서브클래스와 이름이 충돌할 염려가 있을 때 뿐이다.
```python
class ApiClass(object):
    def __init__(self):
        self._value= 5
    
    def get(self):
        return self._value
        
class Child(ApiClass):
    def __init__(self):
        super().__init__()
        self._value = 'hello'
        
a = Child()
print(a.get(), 'and', a._value)

>>>
hello and hello
```

이런 충돌은 속성 이름이 value처럼 아주 일반적일 때 일어날 확률이 특히 높다.
위험을 줄이려면 부모 클래스에서 비공개속성을 사용해서 자식클래스와 속성이름이 겹치지 않게 하면된다.(__value)

> 파이썬 컴파일러는 비공개 속성을 엄격하게 강요하지 않는다.  
> 서브 클래스가 내부 API와 속성에 접근하지 못하게 막기보다는 처음부터 내부 API와 속성을 더 많은 일을 할 수 있게 설계하자.  
> 비공개 속성에 대한 접근을 강제로 제어하지 말고 보호 필드를 문서화해서 서브클래스에 필요한 지침을 제공하자.  
> 직접 제어할 수 없는 서브클래스와 이름이 충돌하지 안헥 할 때만 비공개 속성을 사용하는 방안을 고려하자.
