---
title: "리스트 반환보다는 제너레이터를 고려하자"
tag : Python
---

```python
def index_wors(text):
    result = []
    if text:
        result.append(0)
    for index, letter in enumerate(text):
        if letter == ' ':
            result.append(index + 1)
    return result

address = 'Four score and seven years age...'
result = index_words(address)
print(result[:3])

>>.
[0, 5, 11]
```

빈 공백의 위치를 리스트화해서 반환하는 이 함수에는 두 가지 문제가 있다.

1) 코드가 약간 복잡하고 깔끔하지않다. -> generator를 통해 정리 할 수 있다

```python
def index_wors(text):
    if text:
        yield 0
    for index, letter in enumerate(text):
        if letter == ' ':
            yield index + 1

address = 'Four score and seven years age...'
result = list(index_words(address))
print(result[:3])

>>.
[0, 5, 11]
```

2) 반환하기 전에 모든 결과를 리스트에 저장해야하므로 메모리가 고갈될 수 있다.
  * 메모리 문제에 있어서는 무조건 제너레이터를 활용하자.
```python
def index_wors(text):
    offset = 0
    for line in handel:
        if text:
            yield offset
        for letter in text:
            offset += 1
            if letter == ' ':
                yield index + 1
 with open('/tmp/address.txt', 'r') as f:
     it = index_file(f)
     results = islice(it, 0, 3)
     print(list(result))

>>.
[0, 5, 11]
```
