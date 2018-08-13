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



```liquid
jekyll serve

---
layout: archive
permalink: /categories/
title: "Posts by Category"
author_profile: true
---
{% include group-by-array collection=site.posts field="categories" %}
{% for category in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ category | slugify }}" class="archive__subtitle">{{ category }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
```


```abap
jekyll serve
```
[codingconventionwiki]:https://en.wikipedia.org/wiki/Coding_conventions
