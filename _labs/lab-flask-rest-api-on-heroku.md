---
title: Lab -- Flask ML API on Heroku
order: 7
---

Learning objectives: at the completion of the next few labs, have done all of
the following:

1. Fit a ML model pipeline, and pickle it.
2. Create a Flask app that loads the pickled model pipeline.
3. Create a Flask _route_ that receives a JSON request with new data, and returns
   a prediction for the new data fed against the model.
4. Run and test the flask app locally.
5. Deploy the flask app to a heroku dyno, and test it there.

## Learning objectives for this mini-lab:

1. Fit a ML model pipeline, and pickle it.
1. Save some newdata to a file that you can use later for testing your
   deployed model
1. Ambidextrously use both jupyter notebooks and straight python files.

Keep in mind!

- Do all work for this and the next few labs in a virtual environment.
- Each time you need to install a new python package within your environment,
  add the package to a file called `requirements.txt`

The examples below will use the creditcard fraud dataset, but
**for your deliverable, you should use your own project dataset**.

## Fit a model

For no good reason other than that I say so,
**use a jupyter notebook for this model-fitting step.**

[Install it into your virtual environment using pip](https://jupyter.org/install).
See the same page for launching JupyterLab. These notebooks are run _within your virtual environment_. This means that they only have access to whatever packages you have installed
within your environment. If you `pip install` new packages into your environment,
your already-running notebooks should be able to find them.

Be kind to your future self, and to other ML noobs: add instructions to a
`README.md` in the root of your repo for how to launch a notebook.


### load your dataset

Remember to load your dataset from a publicly-accessible place. Even though you'll
be deploying a pre-fitted model, we want anyone to be able to re-run the model-fitting
code for this deliverable, for the sake of open science.

### fit a pipeline

See the example on this page: <https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html>. It fits a simple pipeline, using `StandardScaler` and a [`SVC`](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html). But SVC doesn't have `predict_proba`, so in your code, switch it for something that does,
such as a [`LogisticRegression`](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html).

Fit a pipeline on your loaded data.

### save the fitted pipeline/model to file by pickling it

I got here by internet-searching "python pickle", first hit: <https://docs.python.org/3/library/pickle.html>. The page is voluminous and I'm lazy, so I look in the nav bar for
an "Examples" section, which takes me [here](https://docs.python.org/3/library/pickle.html#examples).

Pickle your fitted pipeline to a file. Save the file in the root of your project
directory. For my review purposes and no other, give your pickled model the name
`model.pkl`.


### Get some testing data

To run new data against your pickled model, you need _new data!_. fitted models'
prediction functions expect a list of lists (2 dimensions), and they return a 2-d list of two predictions per record -- one prediction per class.

Save at least two rows from your data to a file. I like saving my data as plain
lists. If you're starting from pandas data, you can select two rows of your choosing
using something like `iloc`, and get their underlying numpy arrays by calling `.values`
on them, and then calling `tolist()` on the return of `.values`. For my review purposes,
call your file `newdata.py`.

#### Loading approach one: Copy-paste, assign to a variable, import the variable

I copy-and-paste the output of these from the jupyter
notebook into my new `newdata.py` file, assigning the values to a variable.
Assigning to a variable will let me `import` that variable from other files later.
Something like this:

```python
# newdata.py
# copy-paste a compatible list of values here, and assign it to a variable
newdata = [ 1, 0, 0, 1, 0, 1 ]
```

Then, from another file:
```python
# anotherfile.py
from newdata import newdata
# `newdata` will be the list from `newdata.py`
```

#### Loading approach two: `json.dump` and `json.load`

You could also avoid copy-pasting, and instead, use the `json.dump` package to dump
your newdata to a file, and `json.load` to read it into a different file. Like so:

```python
# file one:
import json

# below, `newdata` might be a few records selected from your dataset
with open('newdata.py', 'w') as f:
  json.dump(newdata, f)
```

From another file:
```python
import json

with open('newdata.py', 'r') as f:
  newdata = json.load(f)
```

### Test that you can load and use the pickled pipeline.

**For no good reason other than that I say so,** create a new python file
(_not_ a new jupyter notebook!) in the root of your repository. For my review purposes
and no other, call your new file `test_pickled_model.py`.

In this file, unpickle your `model.pkl`, and run `predict_proba` against it using
your newdata. You can import your `newdata` variable from `newdata.py` using an approach like this:

```python
# test_pickled_model.py

newdata = ____ # load your newdata, see sections above...

pipe = ____ # unpickle the pipe...

# `model.predict` and friends expect a 2-d array of data. If your newdata is
# one-dimensional, make it two-dimensional by wrapping it in a list.
# numpy can be used to check the number of dimensions of a list.
#    
#    import numpy as np
#    newdata = np.array(newdata)
#    if newdata.ndim == 1:
#        newdata = [newdata]

predictions = pipe.predict_proba(newdata)
print(predictions)
```

Run your test python file by invoking it from a shell that has your environment
active. For instance, if my environment were called `pickled-piper`, me runnning
the file might look like this:

    (deploy-ml) /projects/deploy-ml $ python test_pickled_model.py

## Deliverable

Using your dataset, create a github repository with the following files:
- `README.md`

  This file should:
  * describe your repository
  * tell users how to install all requirements using pip
  * tell users how to fit a model
  * tell users how to use the test file
- `.gitignore`
- `requirements.txt`

  Should include each package necessary to run your notebook and test file
- `fit-model.ipynb`
  - Should be able to load your dataset if run by an anonymous user
  - should fit a pipeline / model using your data, using any classifier you choose
  - should pickle the model
- `newdata.py` or `newdata.json` depending on your chosen approach

  Should contain data that is compatible with your pickled model (should be able to be fed to `predict`)
- `test_pickled_model.py`
  Should load your test data and feed it to your unpickled model to get a prediction.
  Should print the prediction.

### Future lab

// create a flask app that loads the pickled model

// create route in your flask app that receives a POST json request with new data,
// and that returns a prediction for that new data

// Write a script that tests that the flask app works when run locally.

// Deploy your app to heroku. Write a script that tests that the flask app works on heroku.
