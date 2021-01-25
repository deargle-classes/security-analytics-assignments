---
title: "Why is fit_transform only here once?"
id: "fit-transform-why"
---

A student asked me, in summary, what the whole "fit_transform" thing is here -- isn't fitting and transforming only for predictive models?
They showed a snippet from one of the kaggle-learn courses.

> Why is the training data hit with fit_transform?
>
> ![kaggle-learn-snippet-fit-transform]({{ 'questions_and_answers/kaggle-learn-snippet-fit-transform.png' | relative_url }})

This is a really key thing to learn about sklearn's api, and why sklearn is just so so so much better than R for machine learning

Pretty much everything in sklearn, by convention, has to have a fit and fit_transform api, in order to be part of the sklearn family. Think broadly about a ML model from an abstract perspective--

First, a shell model is created. Nascent, empty, blank baby, but a shell nonetheless.

That's commonly what happens whenever an instance of a sklearn class is instantiated -- that's the capital-letter class name called via ()

Ultimately, the goal is to use the new instance to do something. For a model, the goal is to make predictions. Before that can be done, the model / shell needs to be... fit. trained. Whatever synonym.

Only after it has been fit does it know how to do whatever it is it is going to be asked to do. In sklearn, you ask a... thing... to do something via calling transform on it

If you tried to call transform without having fit an instance, it will throw an exception (crash)

So, the fit_transform above is a convenience function, that both uses the training data to fit the imputer-machine. And then, and this is very important, anything you do to change the testing data, you have to do it to the training data, too. And to whatever else you're going to ask your model to make predictions for in the future. Has to be mirrored. We'll see more about that later.

The abstract thing to learn, is that imputers, onehotencoders, etc, all follow that paradigm. Fit it, then do something with it, to change the test/train data.

It's not "fair" though to fit your machines on anything except the training data. No testing data allowed. To do otherwise is what is sometimes called "target leak." The model shouldn't, at any stage, have insight into the data it will be tested against.

One other thing from that snippet above. Sklearn is a punk in that it hates pandas-y things like column names. It uses, under the hood, numpy arrays, which coincidentally is the same thing that pandas builds on top of. So a lot of sklearn things can take a pandas df as input, but they're jerks and will extract out the numpy array and only return a numpy array to you.

Or, it will transform it back into a pandas df before passing it back to you, but it doesn't do the common courtesy of adding the columns back that it dropped. Jerks.

Now, what makes sklearn / python so much better for ML than R -- it's hat fit_transform api. everything has it. All possible modeling approaches. Which means it's generally totally possible to try like a dozen different modeling approaches without needing to know specifics of how to call some library.

They'll all have fit_transform. And they'll all take a pandas df. That is not the case at all in R.

> So, the SimpleImputer() in sklearn even though it only imputes the mean value of its current column into missing values of the same column needs to be "fit" to the training data? Why would the same function of imputation not throw a code on the validation data? I know it shouldn't be fit to any form of testing but that itself is confusing me on why it's required at all.

But with what value will it replace missing fields?

If the mean, then it has to memorize that value, to be used during the actual transforming. once it's fit, the shell remembers it. It only has to be called once in the lifetime of a given class instantiation

The imputer object is updated in-place

With the sklearn approach, you don't have to save the return value from fitting the object. The object itself is updated, you don't need a new reference to the newly fitted object
