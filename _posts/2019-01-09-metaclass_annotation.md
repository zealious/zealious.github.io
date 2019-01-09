---
title: "메타클래스로 클래스 속성에 "
tag : 
   - Python 
---

메타클래스로 구현할 수 있는 기능 중 하나는 클래스를 정의한 이후에,  
하지만 그 클래스를 실제로 사용하기 전에 프로퍼티를 수정하거나 주석을 붙이는 것이다.  

이 기법을 디스크립터와 함께 사용하여 클래스에서 어떻게 사용하는지 자세히 조사한 정보를 디스크립터에 제공한다.

```python
class Field():
    def __init__(self, name):
        self.name = name
        self.internal_name = '_' + self.name
        
    def __get__(self, instance, instance_type):
        if instance is None: return self
        return getattr(instance, self.internal_name, '')
    
    def __set__(self, instance, value):
        setattr(instance, self.internal_name, value)
```

Field 디스크립터에 저장할 컬럼 이름이 있으면 내장 함수 setattr과 getattr을 사용해서  
모든 인스턴스별 상태를 인스턴스 딕셔너리에 보호 필드로 직접 저장할 수 있다.

처음에는 이 방법이 메모리 누수를 피하려고 weakref로 디스크립터를 만드는 방법보다 훨씬 편리해 보인다.  

로우를 표현하는 클래스를 정의할 때는  각 클래스 속성에 대응하는 칼럼의 이름을 지정해야한다.

```python
class Customer():
    first_name = Field('first_name')
    last_name = Field('last_name')
    prefix = Field('prefix')
    suffix = Field('suffix')

foo = Customer()
print('Before:', repr(foo.first_name), foo.__dict__)
foo.first_name = 'Euclid'
print('After: ', repr(foo.first_name), foo.__dict__)

>>>
Before: '' {}
After: 'Euclid' {'_first_name': 'Euclid'}
```

class문 본문에서 Field 객체를 생성하여 Custormer.first_name에 할당할 때 필드의 이름을 선언했다.  
왜 필드 이름(여기서는 'first_name')을 Field생성자에도 넘겨야 할까?

문제는 Customer 클래스 정의에서 연산 순서가 왼쪽에서 오른쪽으로 읽는 방식과는 반대라는 점이다.
먼저 Field생성자는 Field('first_name') 형태로 호출한다. 다음으로 이 호출의 반환 값을 Customer.field_name에 할당한다.  
그러므로 Field에서는 자신이 어떤 클래스 속성에 할당될지 미리 알 방법이 없다.
  
메타클래스를 사용하여 문제를 해결하자.  
메타클래스를 이용하면 class문을 직접 후킹하여 class 본문이 끝나자마자 원하는 동작을 처리할 수 있다.
  
아래 예제에서는 필드 이름을 수동으로 여러번 지정하지 않고 메타클래스를 사용하여 Field.name과 Field.internal_name을  
디스크립터에 자동으로 할당된다.
```python
class Meta(type):
    def __new__(meta, name, bases, class_dict):
        for key, value in class_dict.items():
            if isinstance(value, Field):
                value.name = key
                value.internal_name = '_' + key
        cls = type.__new__(meta, name, bases, class_dict)
        return cls
        
class DatabasesRow(object, metaclass=Meta):
    pass
```
메타클래스를 사용하게 해도 필드 디스크립터는 변경이 거의 없다.
유일한 차이는 더는 생성자에 인수를 넘길 필요가 없다는 점이다.  
대신 필드 디스크립터의 속성은 위의 Meta.__new__메서드로 설정된다.

> 메타클래스를 이용하면 클래스가 완전히 정의되기 전에 클래스 속성을 수정할 수 있다.  
> 디스크립터와 메타클래스는 선언적 동작과 런타임 내부 조사(introspection)용으로 강력한 조합을 이룬다.
> 메타클래스와 디스크립터를 연계하여 사용하면 메모리 누수와 wekref모듈을 모두 피할 수 있다.
