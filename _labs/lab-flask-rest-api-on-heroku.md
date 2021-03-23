---
title: Lab -- Flask ML API on Heroku
order: 7
---

Draft!

In this lab, you'll x all the y:

1. Fit a ML model pipeline, and pickle it.
2. Create a flask app that loads the pickled model pipeline.
3. Create a flask _route_ that receives a JSON request with new data, and returns
   a prediction for the new data fed against the model.
4. Run and test the flask app locally.
5. Deploy the flask app to a heroku dyno, and test it there.

Do all of the above in your "deploy ML" project / environment / repository you
created in the previous lab.

Each time you need to install a new python package, add it to a file called `requirements.txt`

// load the credit card fraud dataset

// fit a simple pipeline, using StandardScaler and a simple LogisticRegression

// save the fitted pipeline/model to file by pickling it

// create a flask app that loads the pickled model

// create route in your flask app that receives a POST json request with new data,
// and that returns a prediction for that new data

// Write a script that tests that the flask app works when run locally.

// Deploy your app to heroku. Write a script that tests that the flask app works on heroku.
