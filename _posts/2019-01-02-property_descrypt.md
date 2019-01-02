---
title: "재사용 가능한 @property 메서드에는 디스크립터를 사용하자"
tag : 
   - Python 
---

파이썬에 내장된 @property의 큰 문제점은 재사용성이다.  
다시말해 @property로 데코레이트하는 메서드를 같은 클래스에 속한 여러 속성에 사용하지 못한다.  
또한 관련 없는 클래스에서도 재사용할 수 없다.  

예를들어 math, writting, science의 등급을 매긴다고할 떄 property를 사용하게되면 코드가  
굉장히 길어진다. 이런 경우 디스크립터를 사용하는 것이다.

```python
class Grade():
    def __init__(self):
        self._value = 0
    def __get__(self, instance, instance_type):
        return self._value
    
    def __set__(self, instance, value):
        if not (0 <= value <= 100):
            raise ValueError('Grade must be between 0 and 100')
        self._value = value
        
class Exam():
    math_grade = Grade()
    writing_grade = Grade()
    science_grade = Grade()

exam = Exam()
exam.writing_grade = 40

# 위코드는 다음과 같이 해석된다.
# Exam.__dict__['writing_grade'].__set__(exam, 40)

print(exam.writing_grade)

## print(Exam.__dict__['writing'].__get_-(exam, Exam))
```

위코드를 아래와 같이 실행해보면 불행히도 제대로 동자갛지 않을 것이다.
한 Exam 인스턴스에 있는 여러 속성에 접근하는것은 기대한대로 동작할것이다.
```python
first_exam = Exam()
first_exam.writing_grade = 82
first_exam.science_grade = 92

print('Writing', first_exam.writing_grade)
print('Science', first_exam.science_grade)
>>
Writing 82
Science 99

# 하지만 여러 Exam 인스턴스의 이런 속성에 접근하면 기대하지 않은 동작들을 하게 된다.
second_exam = Exam()
second_exam.writing_grade = 75
print('Second', second_exam.writing_grade, 'is right')
print('First ', first_exam.writing_grade, 'is wrong')
>>>
Second 75 is right
First  75 is wrong
```

문제는 한 Grade 인스턴스가 모든 Exam 인스턴스의 writing_grade 클래스 속성으로 공유된다는 점이다.  
이 속성에 대응하는 Grade인스턴스는 프로그램에서 Exam인스턴스를 생성할 때마다 생성되는게 아니라 Exam 클래스를 처음 정의할때 한번만 생성된다.

이 문제를 해결하려면 각 Exam 인스턴스별로 값을 추적하는 Grade클래스가 필요하다.
```python
class Grade():
    def __init__(self):
        self._value = {}
    def __get__(self, instance, instance_type):
        if instance is None: return self
        return self._value.get(instance, 0)
    
    def __set__(self, instance, value):
        if not (0 <= value <= 100):
            raise ValueError('Grade must be between 0 and 100')
        self._value[instance] = value
```
이 구현도 잘 동작하지만 여전히 문제점이 하나 남아있다. 바로 메모리 누수다.
_values 딕셔너리는 프로그램의 수명동안 __set__에 전달된 모든 Exam 인스턴스의 참조를 저장한다.  
결국 인스턴스의 참조 개수가 절대로 0이 되지않아 가비지 컬렉터가 정리하지 못하게 된다.

파이썬의 내장모듈 weakref를 사용하면 이 문제를 해결할 수 있다.  
이 모듈은 _values에 사용한 간단한 딕셔너리를 대체할 수 있는 WeakKeyDictionary라는 특별한 클래스를 제공한다.
고유 동작은 런타임 마지막으로 남은 Exam 인스턴스의 참조를 갖고 있다는 사실을 알면 키 집합에서 Exam 인스턴스를 제거하는 것이다.
```python
class Grade():
    def __init__(self):
        self._value = WeakKeyDictionary()
    # ...
```

> 직접 디스크립터 클래스를 정의하여 @property 메서드의 동작과 검증을 재사용하자.  
> WeakKeyDictionary를 사용하여 디스크립터 클래스가 메모리 누수를 일으키지 않게 하자.  
> __getattribute__가 디스크립터 프로토콜을 사용하여 속성을 얻어오고 설정하는 원리를 정확히 이해하려는 함정에 빠지지말자
> 
