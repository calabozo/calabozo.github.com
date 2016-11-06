---
layout: default
title: Basic tools for data scientists
excerpt: R, spark, python 
search_omit: false
---

<ul class="post-list">
{% for tool in site.data.tools %}
  <li><article>
<h2><img src="{{ tool.img }}" style="height:40px;margin-right:0.5em">{{ tool.name }}</h2>
<span class="excerpt" >site: <a href="{{ tool.url }}">{{ tool.url }}</a></span><br/>
<div>{{ tool.desc }}</div><br/>
{% if tool.tutorial %}
<span><a href="{{ tool.tutorial.url }}">{{ tool.tutorial.desc }}</a></span><br/><br/>
{% endif %}

{% if tool.links %}
<div>More resources:</div>
<ul class="simple-list">
{% for link in tool.links %}
<li>
<span>&nbsp;&nbsp;<a href="{{ link.url }}">{{ link.name }}</a>: {{ link.desc }}</span>
</li>
{% endfor %}
</ul>
{% endif %}

</article></li>
{% endfor %}
</ul>


