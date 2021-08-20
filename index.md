---
---

# Labs

{% assign labs = site.labs | sort: "order" %}
{%- for lab in labs %}
   * [ {{ lab.title }} ]( {{ lab.url | relative_url }})
{%- endfor %}

# Pages

* [Questions and Answers]({{ '/q-and-a' | relative_url}})
* [Datasets]({{ '/datasets' | relative_url }})
* [Random]({{ '/random-wisdom' | relative_url }})
* [CRISP-DM report writing]({{ '/crisp-dm-report-format' | relative_url }})
* [Exam Images]({{ '/exam-images' | relative_url }})
* [Hybrid Analysis]({{ '/hybrid-analysis-overview' | relative_url }})

# Mini Lessons

* [Dealing with Unbalanced Datasets](https://colab.research.google.com/drive/1kUWUFGhZVpoPS7nVVJowl0md49wKP48i?usp=sharing)
* [Standardizing and Normalizing](https://colab.research.google.com/drive/1Km5p17IZ_aCOCMe4WqQTKcdvMwrvfLEi?usp=sharing)

### Times I've taught this course

- [MSBX5500 Spring 2021 class website](https://classes.daveeargle.com/msbx5500-spring-2021/)
- [MSBX5500 Spring 2020 class Github repo](https://github.com/deargle-classes/msbx5500-spring-2020)
