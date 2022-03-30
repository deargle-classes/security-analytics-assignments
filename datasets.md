---
title: Security-related ML Datasets
layout: page
include_toc: true
---

This page documents some security-related Machine Learning-friendly datasets that I have found. I might might some of these for assignments in this class.

Note that sklearn has [`sklearn.dataset.fetch_openml`](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_openml.html) which can be used to fetch any of the datasets below that are on
OpenML.

[Here is a jupyter notebook I wrote demonstrating fetching various security datasets](https://github.com/deargle/deargle.github.io/blob/master/notebooks/ml_datasets_examples.ipynb).


{% for dataset in site.datasets %}
<h1 id='{{ dataset.slug }}'>{{ dataset.title }}</h1>
<div>{{ dataset.content | markdownify }}</div>
{% endfor %}
