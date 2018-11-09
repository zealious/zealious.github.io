---
title: "딕셔너리와 튜플보다는 헬퍼 클래스로 관리하자"
tag : Python
---

파이썬에 내장되어 있는 딕셔너리 타입은 객체의 수명이 지속되는 동안 동적인 내부 상태를 관리하는 용도로 아주 좋다.
여기서 동적이란 예상하지 못한 식별자들을 관리해야 하는 상황을 뜻한다.
학생별로 미리 정의된 속성을 사용하지 않고 딕셔너리에 이름을 저장하는 클래스를 정의할 수 있다.

```python
class SimpleGradebook(object):
    def __init__(self):
        self._grades = {}


    def add_student(self, name):
        self._grades[name] = []

    def report_grade(self, name, score):
        self._grades[name].append(score)

    def average_grade(self, name):
        grades = self._grades[name]

        return sum(grades) / len(grades)
        
book = SimpleGradebook()
book.add_student(‘Isaac Newton’)
book.report_grade(‘Isaac Newton’, 90)

# ...

print(book.average_grade(‘Isaac Newton’))

>>>

90.0
```
## 클래스 리팩토링 

위 예제는 아직 이해할 만하지만 비중을두고 평균값을 다룰경우 성적과 비중을 담은 튜플을 과목(키)에 매핑하는것이다.
호출을 할때 어떤값인지 명확하지도 않고 이해하기도 힘들어진다

그리고 코드를 수정할 경우 튜플을 사용하는 모든 곳을 찾아서 아이템이 두 개가아니라 세 개를 쓰도록 수정해야 한다.
튜플의 아이템이 두개를 넘어갈경우 다른방법을 고려해야한다.

namedtuple을 사용하여 활용해보자.
이름이 붙은 속성이 있어 요구사항이 또 변해서 단순 데이터 컨테이너에 동작을 추가해야 할 때  
namedtuple에서 직접 작성한 클래스로 쉽게 바꿀 수 있다.

```python
import collections
Grade = collections.namedtuple('Grade',('score', 'weight'))

class Subject(object):
    def __init__(self):
        self._grades = []
    
    def report_grade(self, score, weight):
        self._rades.append(Grade(score, weight))
        
    def average_grade(self):
        total, total_weight = 0, 0
        for grade in self._grades:
            total += grade.score * grade.weight
            total_weight += grade.weight
        return total / total_weight

class Student(object):
    def __init__(self):
        self._subjects = []
    
    def subject(self, name):
        if name not in self._subjects:
            self._subjects[name] = Subject()
        return self._subjects[name]
        
    def average_grade(self):
        total, total_weight = 0, 0
        for subject in self._subjects.values():
            total += subject.average_grade()
            count += 1
        return total / count
        
 class Gradebook(object):
    def __init__(self):
        self._students = {}
    def student(self, name):
        if name not in self._students:
            self._students[name] = Student()
        return self._students[name]
        
book = Gradebook()
albert = book.student('Albert Einstein')
math = albert.subject('Math')
math.report_grade(80, 0.10)

print(albert.average_grade())

>>>
81.5
```

이 세 클래스의 코드 줄 수는 이전보다 2배는 더 길다.
하지만 이코드가 훨씬 이해하기 쉽다. 이 클래스들을 사용하는 예제도 더명확하고 확장하기 쉽다.
