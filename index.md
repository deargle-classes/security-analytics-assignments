---
---

# Labs

{% assign labs = site.labs | sort: "order" %}
{%- for lab in labs %}
   * [ {{ lab.title }} ]( {{ lab.url | relative_url }})
{% endfor %}

# Pages

* [Questions and Answers]({{ '/q-and-a' | relative_url}})
* [Datasets]({{ '/datasets' | relative_url }})
* [Random]({{ '/random-wisdom' | relative_url }})

### Past times I've taught this course

- [MSBX5500 Spring 2020 class Github repo](https://github.com/deargle-classes/msbx5500-spring-2020)
