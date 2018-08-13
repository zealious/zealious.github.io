---
title: "파이썬 코딩 컨벤션"
tag : Python
---


## Coding Convention  
    
[코딩 컨벤션][codingconventionwiki]이란 개념은 무엇일까?
코딩컨벤션은 프로그램 코드를 작성할때의 기준이라고 볼 수 있다.  
예를들어 들여쓰기(Indentation)을 Space로할지 Tab으로 할지 or 최대 라인 길이 등 다양하다.

이런 기준들을 지키지 않고 개인의 기준대로 코딩을 하게되면 추후에 수정을 하기도 힘들고 공수도 많이 들어간다.  
따라서 개인과 다른 팀원들을 위해 기준을 지켜주면 좋을 것 같다.

지금부터 살펴볼 코딩컨벤션은 파이썬의 [PEP8][PEP8]이다.

## PEP?  

PEP(Python Enhance Proposal)이란 파이썬을 개선하기 위한 개선 제안서입니다.  
이 제안서는 3가지로 구분이 되는데요.  

1) 새로운 기능이나 구현을 제안하는 Standard Track  

2) (구현을 포함하지 않는) 파이썬의 디자인 이슈나 일반적인 지침, 혹은 커뮤니티에의 정보를 제안하는 Informational  

3) 파이썬 개발 과정의 개선을 제안하는 Process  

자세한 내용은 PEP에 대해 다루고 있는 PEP인 [PEP 1][PEP1]을 참고하세요.
파이썬의 코딩 컨벤션을 제안서로 나타내고 있는데 이것이 바로 [PEP 8][PEP8]입니다.

## 불필요한 일관성은 좋지 않을 수 있다.


위에 업급되었던 Coding Convention, PEP8은 일관성 관한 내용입니다.  
그러나 PEP8에 나오는 기준으로만 작성필요는 없습니다. 문서에 아래와 같이 나옵니다.  

> A Foolish Consistency is the Hobgoblin of Little Minds  
  멍청하게 일관성을 유지하는것은 소인배의 발상이다.

우리가 정한 기준은 있지만 때로는 그 기준을 따라갈 필요가 없다는 얘기입니다.  
예외에대한 4가지 정도의 기준이 있는데요. 아마도 1,2번의 경우가 많을 것 같습니다.
> Some other good reasons to ignore a particular guideline:  
>  1. When applying the guideline would make the code less readable, even for someone who is used to reading code that follows this PEP. 
>     PEP8 기준을 잘 알고 있는 사람이라도 읽기 쉬울 경우
>  2. To be consistent with surrounding code that also breaks it (maybe for historic reasons) -- although this is also an opportunity to clean up someone else's mess (in true XP style).  
>     주변코드와 일관성을 위해(옛날부터 작성되었던 기준에 따라가서)
>  3. Because the code in question predates the introduction of the guideline and there is no other reason to be modifying that code.  
>     스타일가이드 도입 이전에 작성되었을경우 수정할 필요가없다??(해석이잘안됨)
>  4. When the code needs to remain compatible with older versions of Python that don't support the feature recommended by the style guide.  
>     스타일 가이드에서 권장하는 기능을 지원하지 않는 이전버전의 파이썬과 호환되도록 해야할 경우



[codingconventionwiki]:https://en.wikipedia.org/wiki/Coding_conventions
[PEP8]:https://www.python.org/dev/peps/pep-0008/
[PEP1]:https://www.python.org/dev/peps/pep-0001/
