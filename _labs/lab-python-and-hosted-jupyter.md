---
title: Lab -- Python for Data Science
learning_objectives:
  - demonstrate basic data science skills with python
---


## Jupyter

Jupyter is everywhere. I want you to try it in several places.


### Jupyter via Anaconda

Anaconda is a python distribution with science-y python libraries pre-compiled and ready-to-go. Very very complicated to compile some of the science-oriented python libraries on Windows otherwise.

Install it!

Then, using Anaconda Navigator, install/launch ~jupyter~ JupyterLab. JupyterLab looks like Jupyter 2.0. Might as well go with the new hotness.

You are now running jupyter on your laptop.


### Jupyter lots of other places

But there are many other places that will let you run jupyter notebooks on their infrastructure. Let's visit and dabble with each of them so that we can say we did.

* [Amazon SageMaker](https://aws.amazon.com/sagemaker/)
  ([Do this tutorial](https://aws.amazon.com/getting-started/tutorials/build-train-deploy-machine-learning-model-sagemaker/))
	* **UPDATE:** When you get to **Step 5a** in the tutorial, the AWS given code will throw an error like this:
		```javascript
		Using already existing model: xgboost-2020-01-28-06-25-08-731
		ResourceLimitExceeded: An error occurred (ResourceLimitExceeded) when calling the CreateEndpoint operation: The account-level service limit 'ml.m4.xlarge for endpoint usage' is 0 Instances,
		```
		According to [this solution](https://stackoverflow.com/questions/53595157/aws-sagemaker-deploy-fails/), the AWS free tier only runs Machine Learning models on instance type = "ml.t2.medium". The given code on the AWS tutorial uses instance type = 'ml.m4.xlarge" which results in the error posted above. To clarify, use **this** code instead for Step 5a in the tutorial:
		``` javascript
		xgb_predictor = xgb.deploy(initial_instance_count=1,instance_type='ml.t2.medium')
		```
* [Google Colab](https://colab.research.google.com)
* [Microsoft Azure Notebooks](https://notebooks.azure.com/)

- [ ] **Deliverable:** Demonstrate somehow that you've used each of these notebook services.

Kaggle also has hosted jupyter notebooks. [Kaggle also has good python-for-datascience tutorials](https://www.kaggle.com/learn/). Do the first seven (!) or so -- up to the TensforFlow one. These will take a while, but should be very good get-feet-wet introductions.

![image](https://user-images.githubusercontent.com/1174653/72477008-de7eab00-37ab-11ea-8806-65867f58ab61.png)

- [ ] **Deliverable:** Show me somehow that you've gone through all the kaggle tutorials. Get as far as you can. Take notes on what you'd like to talk more about.
