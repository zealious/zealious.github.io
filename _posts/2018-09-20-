---
title: "인수를 순회할 때는 방어적으로 하자"
tag : Python
---

```python
def normalize(numbers):
    total = sum(numbers)
    result = []
    for value in numbers:
        percent = 100 * value / total
        result.append(percent)
    return result    
    
visits = [15, 35, 80]
percentages = normalize(visits)

print(percentages)

>>>
[11.538, 26.923, 61.538]
```

만약 대용량 리스트를 받을 경우 메모리를 많이 사용하기 때문에 제너레이터를 사용해야한다.

```python
def read_visits(data_path):
    with open(data_path) as f:
        for line in f:
            yield line

it = read_vistis(path)
percentages = normalize(it)

print(percentages)
```

출력결과가 맨위의 코드와 같이 나올것으로 예상되지만 제너레이터를 사용한 코드의 결과는 아무것도 출력되지않았다.

이런 결과가 나온 건 이터레이터가 결과를 한 번만 생성하기 때문이다.
위 코드를 보면 visits 리스트를 함수인수로 numbers에 대입해 sum과 for문 2군데에서 사용했다.
sum에서 소멸됐기때문에 for문에서는 아무것도 없는값으로 인식된다.

이미 소진한 이터레이터를 순회하더라도 오류가 일어나지 않아 혼란 스러울 수 있고
결과가 없는 이터레이터와 결과가 있었지만 이미 소진한 이터레이터의 차이를 알려주지 않는다.

이 문제를 해결할 수 있는 가장 좋은 방법은 이터레이터 프로토콜을 구현한 새 컨테이너 클래스를 제공하는 것이다.

for x in foo 같은 문장을 만나면 실제로는 iter(foo)를 호출하고 내장함수 iter는 특별한 메서드인 foo.__iter__를 호출한다.
__iter__메서드는 (__netxt__라는 특별한 메서드를 구현하는) 이터레이터 객체를 반환한다. 마지막으로 for루프는 이터레이터를 모두 소진할때까지
이터레이터 객체에 내장함수 next를 계속호출 한다.


클래스의 __iter__메서드를 제너레이터로구현하면 이렇게 동작하게 만들 수 있다.
```python
class ReadVisits(object):
    def __init__(self, data_path):
        self.data_path = data_path
        
    def __iter__(self):
        with open(self.data_path) as f:
            for line in f:
                yield int(line)
                
visits = ReadVisits(path)
percentages = normalize(visits)
print(percentages)

>>>
[11.538, 26.923, 61.538]
```
이 코드는 독립적으로 동작하므로 순회과정에서 모든입력 데이터값을 얻을 수 있다.
유일한 단점은 입력 데이터를 여러 번 읽는다는 점이다.

이제 파라미터가 단순한 이터레이터가 아님을 보장하는 함수를 작성할 차례다.
* 이터레이터를 넘기면 결과값이 제대로 안나오기 때문에 조건을 넣는다.

```python
def normalize(numbers):
    if iter(numbers) is iter(numbers):
        raise TypeError('Must supply a container)
    total = sum(numbers)
    result = []
    for value in numbers:
        percent = 100 * value / total
        result.append(percent)
    return result    
    
class ReadVisits(object):
    def __init__(self, data_path):
        self.data_path = data_path
        
    def __iter__(self):
        with open(self.data_path) as f:
            for line in f:
                yield int(line)
                
visits = ReadVisits(path)
percentages = normalize(visits)

print(percentages)

>>>
[11.538, 26.923, 61.538]
```
