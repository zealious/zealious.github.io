

## range와 enumerate 어떤걸 사용해야할까?

## range란?

* 입력받은 숫자에 해당되는 범위의 값을 반복(iterable) 가능한 객체로 만들어 리턴한다.  
```python
# range([start],stop,[,step])

for i in range(10):
    print(i)
```

## enumerate란?
* 리스트가 있는경우 인덱스와 리스트의 값을 전달하는 역할  
* enumerate는 지연 제너레이터(lazy generaor)로 이터레이터를 감싼다.  
* 지연 제너레이터는 인덱스와 값을 한쌍으로 가져와 넘겨준다.  
```python
for count, name in enumerate(list):
    print(count, name)
```

## 

```python
# range를 활용할 경우
for i in range(len(flavor_list)):
    flavor = favor_list[i]
    print('%d: %s' %(i + 1, flavor))

# enumerate를 활용할 경우
for count, value in enumerate(flavor_list):
    print('%d: %s' %(i + 1, value))
```

## 핵심정리
* range로 루프를 실행하고 시퀀스에 인덱스로 접근하기보다는 enumerate를 사용하는게 좋다.
* enumerate에 두 번째 파라미터를 사용하면 세기 시작할 숫자를 지정 할 수 있다.(기본값은 0)
