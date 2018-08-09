---
layout: splash
permalink: /
header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/background.png
  cta_label: "<i class='fas fa-download'></i> Install Now"
  cta_url:
  caption:

---

<h3 class="archive__subtitle">{{ site.data.ui-text[site.locale].recent_posts | default: "Recent Posts" }}</h3>
hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh
{% for post in paginator.posts %}
  {% include archive-single.html %}
{% endfor %}

{% include paginator.html %}
