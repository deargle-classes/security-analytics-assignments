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


# Scratch

Cut from other labs, pasting here for now...

## Jupyter on Amazon (AWS) SageMaker

AWS is an ungainly beast. Have fun!

The reason to run jupyter on a platform like SageMaker (GCP has one too) is
because you get direct AWS API access to other AS micro-ML resources. You'll see
in the tutorial, but you can directly (yet complexly!) do things like load data
from s3, deploy models to specific hosted endpoints, and save predictions to
_other_ s3 buckets. The curse and the blessing is the modularity.

But it's still Jupyter! You don't _have_ to use the fancy stuff...

Hopefully, later in the semester, we'll get to where you get more experience with these.

[Amazon SageMaker](https://aws.amazon.com/sagemaker/)

([Do this tutorial](https://aws.amazon.com/getting-started/tutorials/build-train-deploy-machine-learning-model-sagemaker/))

**UPDATE:** When you get to **Step 5a** in the tutorial, the AWS given code will throw an error like this:

```javascript
Using already existing model: xgboost-2020-01-28-06-25-08-731
ResourceLimitExceeded: An error occurred (ResourceLimitExceeded) when calling the CreateEndpoint operation: The account-level service limit 'ml.m4.xlarge for endpoint usage' is 0 Instances,
```
According to [this solution](https://stackoverflow.com/questions/53595157/aws-sagemaker-deploy-fails/), the AWS free tier only runs Machine Learning models on instance type = "ml.t2.medium". The given code on the AWS tutorial uses instance type = 'ml.m4.xlarge" which results in the error posted above. To clarify, use **this** code instead for Step 5a in the tutorial:

``` javascript
xgb_predictor = xgb.deploy(initial_instance_count=1,instance_type='ml.t2.medium')
```

**Heads up!** Be _very very very careful_ to delete any resources you created.
Follow the cleanup steps carefully. Otherwise, you will get an unhappy bill.
Customer service will probably waive it for you. But if that happens, be grateful -- getting an unexpected AWS bill is part of the cultural AWS user experience.

## Now -- Jupyter on GCP, the docker way

Docker exists so that you don't have to install a bunch of stuff. Docker is
therefore DevOps-y.

There are excellent Jupyter Docker containers. You've already used one in your
Unstructured Data class, but you need to use _moar_ of them. _Moar!_

Check out [the jupyter docker stacks image selection
page](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html)
and browse their different available docker images. You'll note that some of
those have some sweet packages.

The further down the list you go, the more packages come pre-installed in the
image. But also, the bigger the image becomes, so the longer it takes to
download.

Which one is your favorite! Which one do you choose! I arbitrarily decide that you choose [the `jupyter/datascience-notebook` one](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook).

* Begin to create a new cloud compute instance.
* Choose an instance with kind-of-good resources.
* For "Container," Click "Deploy Container," and enter the name of the docker container you want -- `jupyter/datascience-notebook`
* For "Boot Disk," click "Change," and give yourself some disk -- the bigger the disk, the faster your IO speeds are.
  So take more disk that you think you need for actual disk-space-ing.
* Create a firewall rule that opens up all traffic for all instances to port 8888

Do _nothing else_ -- no arguments or anything to the container.

* You ask: "Huh? Don't we need to `--publish` the port, so that we can access it from the internet?"

  No you do not! GCP launches containers in the Docker `--network=host` mode. This means _all_ ports within the container
  are directly accessible from the host, without any additional configuration.

* You ask: "Huh? Don't we need to mount a volume?"

  No you do not! Your notebooks and whatever will be stored within the container, but you can use the jupyter interface to
  upload/download.

Keep an eye on the serial logs to see when the container has finished pulling. It might take a minute. Before it's done,
you'll see this:

* From the GCP "VM instances" dashboard, click your instance name,
* then click "Serial port 1 (console)"
* scroll down to see the progress. It will sit in "pulling" for up to a few
  minutes, after which you'll see a lot of logs from the pulling process.

  After it's done, you'll see something like this:

* Now, visit your instance's public IP address, on port 8888.

:tada: Jupyter is live! But wait, what is the access key?

## Get the access key

To get the key, use ssh-in-the-browser to connect to the instance.

GCP launched the container as a docker image,
so we can find the instance using the `docker ps` command:

```bash
docker ps
```

Jupyter logs its token on startup. So let's use docker to view the container's logs, using its name from the previous step:

```bash
docker log name
```

There's the token!

Copy the link, and paste it into a text editor or something so that you can get _just the token_ (GCP by default will copy more to the clipboard than you think).

Paste the token into the login page. :tada: you should be in!

Was that easy? Let's review the steps:

1. You used the GCP gui to create a new instance
   * You specified the container name
   * You bumped up the disk space
   * You enabled http
1. You created the instance.
1. You waited around while it pulled the docker image
1. You ssh logged into the image
   * You got the docker container's name
   * You read the docker container's logs
   * You copied the token from the logs
1. You submitted the token.
1. You clapped with joy.

Oh and,
1. You remembered to delete the instance.

Look at you _flexing_.


# Use your custom image on GCP

Say you now wanted to use your modified Docker image as a container on a gcp instance.
To do this, you need to **push your image to a public docker repo**. It's not hard!

We'll use the DockerHub repo:
1.  In your browser, create or log into an account on DockerHub.

From a terminal on your workstation in the directory of your repo:
1.  Tag the image:
    ```bash
    ```
1.  Push it:
    ```bash
    ```
1.  In your browser, visit dockerhub, and confirm that your image has been uploaded. **Note the image's name**.
1.  Your dockerhub image should include a link back to your github repo.

Now, create an image on GCP using your image:

1. Repeat the earlier section for GCP, this time providing the name of your customized docker image.
   * Modify the command-line-equivalent that you created at the end of step xyz to use your custom image.
1. Confirm that you can get a browser with jupyter for your instance.
1. Confirm that your packages are available.

Holy smokes, you did it. You are l33t.

Add your modified gcp command to your README.md. Make sure that it is formatted
properly for code.


## Do the same, with Vertex AI "notebook" stuff

Now you'll do the above even _more_ easily with GCP, except with what I argue is too much
magic.

Todo: steps for Vertex AI Notebooks
