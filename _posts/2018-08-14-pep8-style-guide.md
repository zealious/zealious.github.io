---
title: "PEP8 Style Guide"
tag : Python
---


## PEP8 Style Guide

파이썬의 [코딩컨벤션][cv]이 PEP8 - Style Guide다.  
이 내용은 파이썬 [공식문서][pep8] 내용을 번역한 내용입니다.  
틀린부분이 있으면 언제든 지적 부탁드립니다.

## Indentation

들여쓰기는 4개의 공백을 사용합니다.  
[hanging indent][hangingindent] 를 사용하는 경우 수직으로 정렬해야합니다.  
첫번째 줄과 연속된 줄로 명확하게 구분하기 위해 들여쓰기를 사용해야합니다.

> hanging indent : 첫번째 행을 제외하고 단락의 모든 행을 들여쓰는 스타일


올바른 예)
```yml
# Aligned with opening delimiter.(여는 괄호를 기준으로 정렬됩니다)
howoo = long_function_name(var_one, var_two,
                           var_three, var_four)

# More indentation included to distinguish this from the rest.(나머지와 구별하기위해 더 많은 들여쓰기를 합니다.)
def howoo_function(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# Hanging indents should add a level.
howoo = howoo_function(
    var_one, var_two,
    var_three, var_four)
```

잘못된 예시)

```yml
# Arguments on first line forbidden when not using vertical alignment.(세로정렬을 하지 않을떄는 첫번째라인의 인자는 금지된다)
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# Further indentation required as indentation is not distinguishable.(들여쓰기를 구분할 수 없을때 들여쓰기가 더 필요하다)
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

### if문의 조건문이 길 경우
다음 줄이 4칸의 공백을 들여쓴다면 아래 코드와 시작적인 충돌이 생길 수 있습니다.  
PEP는 명시적으로 어떻게 하라는 입장을 취하지 않았습니다.  
3가지 정도의 예시가 있습니다.

```yml
# No extra indentation.
if (this_is_one_thing and
    that_is_another_thing):
    do_something()

# Add a comment, which will provide some distinction in editors
# supporting syntax highlighting.
if (this_is_one_thing and
    that_is_another_thing):
    # Since both conditions are true, we can frobnicate.
    do_something()

# Add some extra indentation on the conditional continuation line.
if (this_is_one_thing
        and that_is_another_thing):
    do_something()
```

### 여러줄 구조체를 닫는 괄호/중괄호/대괄호는 다음과같이 할 수 잇습니다.

```yml
1) 첫번째 공백이 아닌 문자아래에 정렬될 수 있다.
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]
2) 시작하는 줄의 첫번째 문자 아래에 쓸 수도있습니다.
my_list = [
    1, 2, 3,
    4, 5, 6,
]    
```


## Tab or Spaces

공백은 선호되는 들여쓰기 방법입니다.  
탭은 이미 탭으로 들여쓰여진 코드와 일관성을 유지하기 위해서만 사용해야합니다.  
파이썬 3에서는 탭과 공백을 섞어서 사용하지 못합니다.  
  
파이썬 2에대하 내용은 사용하지않아서 작성하지 않았습니다.

## Maximum Line Length

모든 라인의 최대 길이는 79자 입니다.  
문자열 또는 주석을 처리하려면 72자로 제한합니다.  

일부는 80~100자로 늘려서 사용하는 경우도 있습니다.  
  
긴 줄을 래핑하는 가종 좋은 방법은 괄호, 대괄호 및 중괄호 안에 넣어 여러줄로 나눌 수 있습니다.  
여러줄로 나눌때는 백슬러시를 사용하는것보다 우선시 되어야합니다.  
  
대신 with절을 사용할 경우 백슬러시를 사용할 수 있습니다.  


## 줄바꿈은 연산자 전에 해야하나 후에해야하나?

앞뒤 둘다 허용하나 가독성을 위해 연사자 앞에서 줄바꿈을 한다.

```yml
easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

## Imports

import는 보통 한줄에 하나씩 쓰입니다.
import는 파일의 맨위 주석 및 docstirings 다음 줄, 전역변수와 상수 위에 놓이게 됩니다.
```yml
import os
import sys

#from이 있을경우는 괜찮습니다.
from subprocess import Popen, PIPE
```

import 는 다음과같이 그룹화 되어야합니다.  

> 1) Standard library imports.
> 2)Related third party imports.
> 3)Local application/library specific imports.

각가의 imports 그룹의 사이에 빈공백으로 구분져야합니다.  
절대 경로를 추천합니다. 일반적으로 더 읽기 쉽고 더 나은 동작을 하는 경향이있습니다.(더 나은 오류메세지를 볼 수 있습니다)

```yml
import mypkg.sibling
from mypkg import sibling
from mypkg.sibling import example
```

그러나 길어지게 될 경우 복잡한 패키지를 다룰땐 상대경로 imports는 절대경로 imports에 대안으로 받아들여질 수 있습니다.



[pep8]: https://www.python.org/dev/peps/pep-0008/
[cv]: https://zealious.github.io/about-python-coding-convention/
[hangingindent]: https://www.python.org/dev/peps/pep-0008/#fn-hi
