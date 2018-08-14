---
title: "PEP8 Style Guide"
tag : Python
---


## PEP8 Style Guide

파이썬의 [코딩컨벤션][cv]이 PEP8 - Style Guide다.  
이 내용은 파이썬 [공식문서][pep8] 내용을 번역한 내용입니다.  
틀린부분이 있으면 언제든 지적 부탁드립니다.

# Indentation

들여쓰기는 4개의 공백을 사용합니다.  
[hanging indent][hangingindent] 를 사용하는 경우 수직으로 정렬해야합니다.  
첫번째 줄과 연속된 줄로 명확하게 구분하기 위해 들여쓰기를 사용해야합니다.

> hanging indent : 첫번째 행을 제외하고 단락의 모든 행을 들여쓰는 스타일


올바른 예)
```yml
# Aligned with opening delimiter.(여는 괄호를 기준으로 정렬됩니다)
howoo = long_function_name(var_one, var_two,
                           var_three, var_four)

# More indentation included to distinguish this from the rest.(나머지와 구별하기위해 더 많은 들여쓰기를 합니다.
def howoo_function(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# Hanging indents should add a level.
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```


[pep8]: https://www.python.org/dev/peps/pep-0008/
[cv]: https://zealious.github.io/about-python-coding-convention/
[hangingindent]: https://www.python.org/dev/peps/pep-0008/#fn-hi
