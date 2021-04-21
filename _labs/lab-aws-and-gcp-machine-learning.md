---
title: Lab -- AWS and GCP Machine Learning
order: 10
---

I mentioned in the last lab that machine learning on cloud platforms like AWS
and GCP can be a bunch of razzle-dazzle, and that it needs to be demystified a bit.
Now that you have deployed a model to heroku, 'tis good that you see what the process
would be like on AWS or GCP.

In this lab, you will complete tutorials for machine learning on both AWS and GCP.
Then, for the deliverable, you will choose one of the providers, and deploy your project dataset.

## 1. Complete AWS and GCP machine learning tutorials

### AWS

* AWS Sagemaker: <https://aws.amazon.com/getting-started/hands-on/build-train-deploy-machine-learning-model-sagemaker/>


It is possible to deploy an already-fit sklearn model to sagemaker. See these docs: <https://sagemaker.readthedocs.io/en/stable/frameworks/sklearn/using_sklearn.html#deploy-an-endpoint-from-model-data>

The SKLearnModel constructor `entry_point` argument should point to a python script that is required to implement at least the `model_fn` function, which is
  used to load your model, see [#load-a-model](https://sagemaker.readthedocs.io/en/stable/frameworks/sklearn/using_sklearn.html#load-a-model). The example shows loading from a `joblib` serialized model:

```python
from sklearn.externals import joblib
import os

def model_fn(model_dir):
    clf = joblib.load(os.path.join(model_dir, "model.joblib"))
    return clf
```

Of course, if you `pickle`d your model instead of `joblib`-ing it, you would un-pickle it here instead of using `joblib`.

The model file that you deserialize should be present in the `model_data` .tar.gz file specified to the `SKLearnModel` constructor.



### GCP AI Platform

[Google's AI Platform](https://cloud.google.com/ai-platform) was rebranded from its former name of "GCP Cloud Machine Learning Engine," because sure let's muddle the terms even more, why not.

The up-front GCP tutorials are focused on keras, but there are some [sklearn notebooks hidden in their
github repository](https://github.com/GoogleCloudPlatform/cloudml-samples/tree/master/notebooks/scikit-learn).

Do the [Online Prediction with scikit-learn on AI Platform](https://github.com/GoogleCloudPlatform/cloudml-samples/blob/master/notebooks/scikit-learn/OnlinePredictionWithScikitLearnInCMLE.ipynb) one. (Paste the github notebook
link into <https://nbviewer.jupyter.org> if github keeps failing to render the notebook).

You'll need to change a few things to bring it up to date:

- `.as_matrix()` is no longer a pandas function. Replace it with `.values`.
- when you create the model, specify the latest runtime instead of 1.4 -- 2.4 is the latest as of 4-20-2021. Follow the link and check the versions of sklearn used to see which one matches what you used to serialize it.
- just leave the python runtime blank. The tutorial recommends using v3.5, but lol don't, that's out of date. The default as of 4-20-2021 is v3.7.

Tips:

- You can use the GCP Console (web browser) to see the effect of running your gcloud. In fact, you don't have to do _any_ of the
gcloud commands -- you can do everything from the console instead.
- Google colab notebooks already have the gcp SDK (gcloud, gsutil) installed. You can log in to gcp from a colab notebook
by running `gcloud init` and following the prompts.

## 2. Deploy your own model

Choose either AWS or GCP. Deploy a model using your data to one of these platforms. Take a screenshot somehow showing you able to
make predictions against your deployed model. Create a separate folder in your repository called `{name_of_platform}_deploy` that
contains its own README with instructions for deploying to your platform. The folder should also include any additional
files you needed to deploy to these platforms.
