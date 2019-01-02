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
