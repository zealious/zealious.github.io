---
title: "PEP8 Style Guide 1편"
tag : Python
---


## PEP8 Style Guide

파이썬의 [코딩컨벤션][cv]이 PEP8 - Style Guide다.  
이 내용은 파이썬 [공식문서][pep8] 내용을 번역 및 의역한 내용입니다.  
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

- if문의 조건문이 길 경우

아래와 보기와 같이 실행코드와 같은 들여쓰기로 되어있다면 시각적으로 헷갈려 할 수 도 있습니다.
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

- 여러줄 구조체를 닫는 괄호/중괄호/대괄호는 다음과같이 할 수 잇습니다.  
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

- 앞뒤 둘다 허용하나 가독성을 위해 연사자 앞에서 줄바꿈을 한다.  
```yml
easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

## Imports

- import는 보통 한줄에 하나씩 쓰입니다
```yml
import os
import sys

# from이 있을경우는 괜찮습니다.

from subprocess import Popen, PIPE
```

- import는 파일의 맨위에 작성되지만 주석 및 docstirings(""" """) 다음 줄, 전역변수와 상수 위에 놓이게 됩니다.
import 는 다음과같이 그룹화 되어야합니다.

> 1) Standard library imports.
> 2) Related third party imports.
> 3) Local application/library specific imports.

각가의 imports 그룹의 사이에 빈공백으로 구분져야합니다.  

- 절대 경로를 추천합니다. 일반적으로 더 읽기 쉽고 더 나은 동작을 하는 경향이있습니다(더 나은 오류메세지를 볼 수 있습니다) 
```yml
import mypkg.sibling
from mypkg import sibling
from mypkg.sibling import example
```
 * 길어지게 될 경우 복잡한 패키지를 다룰땐 상대경로 imports를 절대경로 imports에 대신사용해도 됩니다.   
```yml
from . import sibling
from .sibling import example
```

Standard library code는 복잡한 패키지 레이아웃을 피하고 항상 절대경로 import를 사용해야합니다.

묵시적인?? 상대적인?? import는 결코 사용되어서는 안되며 Python 3에서는 제거되었습니다.


## 클래스가 정의된 모듈에서 클래스를 import할 경우 보통 아래 처럼 사용합니다.

```yml
from myclass import MyClass
from foo.bar.yourclass import YourClass
```

만약 이 경우 지역변수가 충돌된다면 아래와 같이 사용합니다.

```yml
import myclass
import foo.bar.yourclass
```

 "myclass.MyClass"  "foo.bar.yourclass.YourClass".와 같이 사용합니다.

## 와일드카드(*) import는 절대 사용하지않는다. (해석이 잘안됨..)
Wildcard imports (from <module> import *) should be avoided, as they make it unclear which names are present in the namespace, confusing both readers and many automated tools. There is one defensible use case for a wildcard import, which is to republish an internal interface as part of a public API (for example, overwriting a pure Python implementation of an interface with the definitions from an optional accelerator module and exactly which definitions will be overwritten isn't known in advance).

## Module Level Dunder Names

__all__, __author__, __version__ 등과 같은 모듈 수준의 "dunders"(즉 앞뒤 두 개의 밑줄이있는 이름)는 모듈 docstring 뒤에 있지만  
__future__ imports를 제외한 모든 import 문 앞에 있어야합니다.   
파이썬은 future-imports는 반드시 모듈에 docstrings을 제외한 다른 코드보다 앞에 나타나야한다고 합니다.

```yml

"""This is the example module.

This module does stuff.
"""

from __future__ import barry_as_FLUFL

__all__ = ['a', 'b', 'c']
__version__ = '0.1'
__author__ = 'Cardinal Biggles'

import os
import sys

```

## String Quotes

파이썬에서는 작은 따옴표로 묶인 문자열과 큰 따옴표로 묶은 문자열이 동일합니다.    
그러나 문자열에 작은 따옴표 나 큰 따옴표가 포함되어 있으면 둘중 하나를 사용하여 문자열의 백 슬래시를 방지하십시오.  
가독성이 향상됩니다.

삼중 따옴표로 묶인 문자열의 경우, 큰 따옴표 문자를 사용하여 [PEP 257][pep257]의 문서 문자열 규칙과 일관되게하십시오.


## Whitespace in Expressions and Statements(표현식과 구문안의 공백)

###Pet Peeves(공치덩어리)

다음과 같은 상황에서 공백이 생기는것을 피한다.

- 괄호(대,중,소) 안의 양쪽 끝
```yml
Yes: spam(ham[1], {eggs: 2})
No:  spam( ham[ 1 ], { eggs: 2 } )
```

- 쉼표와 닫히는 괄호 사이
```yml
Yes: foo = (0,)
No:  bar = (0, )
```

- 콤마,세미콜론,콜론 이전의 공백
```yml
Yes: if x == 4: print x, y; x, y = y, x
No:  if x == 4 : print x , y ; x , y = y , x
```

```
However, in a slice the colon acts like a binary operator, and should have equal amounts on either side (treating it as the operator with the lowest priority). In an extended slice, both colons must have the same amount of spacing applied. Exception: when a slice parameter is omitted, the space is omitted.슬라이스의 콜론은 이항연산자와 같이 양쪽에 같은 양을 가져야합니다(가장낮은 우선순위의 연산자로 처리) 확장된 슬라이스에서 두 콜론은 같은 양의 간격을 적용해야합니다. 다만 피연산자가 누락되었을경우에는 공백을 주지 않습니다  
```
```yml
Yes:

ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
ham[lower:upper], ham[lower:upper:], ham[lower::step]
ham[lower+offset : upper+offset]
ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
ham[lower + offset : upper + offset]

No:

ham[lower + offset:upper + offset]
ham[1: 9], ham[1 :9], ham[1:9 :3]
ham[lower : : upper]
ham[ : upper]
```

- 함수 호출 인수 앞에 공백
```yml
Yes: spam(1)
No:  spam (1)
```

- 인덱싱이나 슬라이싱하는 괄호 바로 앞
```yml
Yes: dct['key'] = lst[index]
No:  dct ['key'] = lst [index]
```

- 대입연산자의 앞뒤는 공백을 주자
```yml
Yes:

x = 1
y = 2
long_variable = 3
No:

x             = 1
y             = 2
long_variable = 3
```


## 기타 권장사항

- Avoid trailing whitespace anywhere. Because it's usually invisible, it can be confusing: e.g. a backslash followed by a space and a newline does not count as a line continuation marker. Some editors don't preserve it and many projects (like CPython itself) have pre-commit hooks that reject it.
- 어디에서나 공백을 피하십시오. 일반적으로 보이지 않으므로 혼란 스러울 수 있습니다. 공백과 개행이 뒤 따르는 백 슬래시는 줄 계속 표시로 간주되지 않습니다. 일부 편집자는 그것을 보존하지 않고 CPython 자체와 같은 많은 프로젝트는 그것을 거부하는 사전 커밋 (pre-commit) 훅을 가지고있다.

- Always surround these binary operators with a single space on either side: assignment (=), augmented assignment (+=, -= etc.), comparisons (==, <, >, !=, <>, <=, >=, in, not in, is, is not), Booleans (and, or, not).
- 할당 (=), 증가 된 할당 (+ =, - = 등), 비교 (==, <,>,! =, <>, <=,> = 등) in, in not, is, is), 불린 (boolean) (and, or not).

- If operators with different priorities are used, consider adding whitespace around the operators with the lowest priority(ies). Use your own judgment; however, never use more than one space, and always have the same amount of whitespace on both sides of a binary operator.
- 우선 순위가 다른 연산자를 사용하는 경우 우선 순위가 가장 낮은 연산자 주위에 공백을 추가하는 것이 좋습니다. 너 자신의 판단을 사용하라. 그러나 둘 이상의 공간을 사용하지 말고 항상 이진 연산자의 양쪽에 같은 공백을 둡니다.

```yml
Yes:

i = i + 1
submitted += 1
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)
No:

i=i+1
submitted +=1
x = x * 2 - 1
hypot2 = x * x + y * y
c = (a + b) * (a - b)

```

- Don't use spaces around the = sign when used to indicate a keyword argument or a default parameter value.

```yml
Yes:

def complex(real, imag=0.0):
    return magic(r=real, i=imag)
No:

def complex(real, imag = 0.0):
    return magic(r = real, i = imag)
    
```

* Function annotations should use the normal rules for colons and always have spaces around the -> arrow if present. (See Function Annotations below for more about function annotations.)

```yml
Yes:

def munge(input: AnyStr): ...
def munge() -> AnyStr: ...
No:

def munge(input:AnyStr): ...
def munge()->PosInt: ...
```

* When combining an argument annotation with a default value, use spaces around the = sign (but only for those arguments that have both an annotation and a default).

```yml
Yes:

def munge(sep: AnyStr = None): ...
def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
No:

def munge(input: AnyStr=None): ...
def munge(input: AnyStr, limit = 1000): ...
Compound statements (multiple statements on the same line) are generally discouraged.

```

```yml
Yes:

if foo == 'blah':
    do_blah_thing()
do_one()
do_two()
do_three()
Rather not:

if foo == 'blah': do_blah_thing()
do_one(); do_two(); do_three()

```
* While sometimes it's okay to put an if/for/while with a small body on the same line, never do this for multi-clause statements. Also avoid folding such long lines!

Rather not:

```yml
if foo == 'blah': do_blah_thing()
for x in lst: total += x
while t < 10: t = delay()
```
Definitely not:
```yml
if foo == 'blah': do_blah_thing()
else: do_non_blah_thing()

try: something()
finally: cleanup()

do_one(); do_two(); do_three(long, argument,
                             list, like, this)

if foo == 'blah': one(); two(); three()
```

[pep8]: https://www.python.org/dev/peps/pep-0008/
[cv]: https://zealious.github.io/about-python-coding-convention/
[hangingindent]: https://www.python.org/dev/peps/pep-0008/#fn-hi
[pep257]: https://www.python.org/dev/peps/pep-0257/
