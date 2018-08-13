---
title: "파이썬 코딩 컨벤션"
tag : Python
---


## Coding Convention

[코딩 컨벤션][codingconventionwiki]이란 개념은 무엇일까요?
코딩컨벤션은 프로그램 코드를 작성할때의 기준이라고 볼 수 있습니다.
```
jekyll serve
```

```yml
jekyll serve
```

{% raw %}
```liquid
---
layout: archive
permalink: /year-archive/
title: "Posts by Year"
author_profile: true
---
{% assign postsByYear = site.posts | group_by_exp:"post", "post.date | date: '%Y'"  %}
{% for year in postsByYear %}
  <h2 id="{{ year.name | slugify }}" class="archive__subtitle">{{ year.name }}</h2>
  {% for post in year.items %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
```
{% endraw %}



```abap
jekyll serve
```
[codingconventionwiki]:https://en.wikipedia.org/wiki/Coding_conventions
