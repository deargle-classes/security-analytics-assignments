---
---

# Labs

{% assign labs = site.labs | sort: "order"  %}
<ul>
{%- for lab in labs %}
{% unless lab.archived %}
<li>
<a href='{{ lab.url | relative_url }}'>{{ lab.title }}</a>
</li>
{% endunless %}
{%- endfor %}
</ul>


# Pages

* [Questions and Answers]({{ '/q-and-a' | relative_url}})
* [Datasets]({{ '/datasets' | relative_url }})
* [Random]({{ '/random-wisdom' | relative_url }})
* [CRISP-DM report writing]({{ '/crisp-dm-report-format' | relative_url }})
* [Exam Images]({{ '/exam-images' | relative_url }})

# Mini Lessons

* [Dealing with Unbalanced Datasets](https://colab.research.google.com/drive/1kUWUFGhZVpoPS7nVVJowl0md49wKP48i?usp=sharing)
* [Scaling and Standardizing](https://colab.research.google.com/drive/1Km5p17IZ_aCOCMe4WqQTKcdvMwrvfLEi?usp=sharing)
* [Netflow Outlier Detection (Anomaly Detection)](https://nbviewer.org/github/deargle-classes/security-analytics-assignments/blob/main/notebooks/anomaly-detection.ipynb)
* [scikit-learn pipeline model evaluation plots (ROC and PR-curve)](https://github.com/deargle/deargle.github.io/blob/master/notebooks/ml_model_evaluation.ipynb)
* [Fetching infosec-related machine learning datasets with scikit-learn](https://github.com/deargle/deargle.github.io/blob/master/notebooks/ml_datasets_examples.ipynb)

# Archived labs
{% assign labs = site.labs | where: "archived", true %}
<ul>
{%- for lab in labs %}
<li>
<a href='{{ lab.url | relative_url }}'>{{ lab.title }}</a>
</li>
{%- endfor %}
</ul>

# Syllabi Links

- [MSBX5500 Spring 2022 class website](https://classes.daveeargle.com/msbx5500-spring-2022/)
- [MSBX5500 Spring 2021 class website](https://classes.daveeargle.com/msbx5500-spring-2021/)
- [MSBX5500 Spring 2020 class Github repo](https://github.com/deargle-classes/msbx5500-spring-2020)
  - This cohort did an incident response flask app where you could upload pcaps to a mongodb, then run `argus / ra` against
    then to get netflows, which were fed against ML models ([repo](https://github.com/deargle-classes/msbx5500-spring-2020-project/), [project planning notes](https://github.com/deargle-classes/msbx5500-spring-2020/blob/master/lab-6.md))

# Other
* [My analytics-related blog posts](https://daveeargle.com/tags#analytics)
