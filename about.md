---
layout: page
title: About
permalink: /about/
published: true
---

<div class="page" markdown="1">

{% capture page_subtitle %}
<img
    class="me"
    alt="{{ author.name }}"
    src="{{ site.author.photo | relative_url }}"
    srcset="{{ site.author.photo2x | relative_url }} 2x"
/>
{% endcapture %}

{% include page/title.html title=page.title subtitle=page_subtitle %}

## Some heading 

안녕. 난 김노아라고 해. 만나서 반갑다. 내가 내돈주고 도메인을 사는 바람에 돈이 아까워서 이것저것 하는중이야. 여기에는 뭘 올릴지는 모르겠지만 아무튼 잘 부탁한다.

</div>
