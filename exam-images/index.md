---
layout: default
include_nav: true
---

<h1>Exam Images</h1>

Canvas (LMS) is being Canvas (derogatory adjective) -- images embedded in reading exams aren't appearing.
So here they are instead.

{% assign quizbanks = site.exam_images | sort: "order" %}

{% for quizbank in quizbanks %}
<h2>{{ quizbank.order }}: {{quizbank.bank}}</h2>

{{ quizbank.content }}
{% endfor %}
