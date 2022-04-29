---
title: Lab -- Deploying to Heroku
order: 9
archived: true
---

Cloud-hosted machine learning platforms such as AWS Sagemaker and whatever GCP calls theirs
need to be demystified. Hence, this lab! In the last lab, you make your pickled
model accessible via a Flask REST json route. This means that you can "deploy"
your machine learning model anywhere you can run a web server. One platform-as-a-service
provider is Heroku. Heroku is owned by Salesforce. They give you a way to launch web-accessible apps that run on
AWS, without you ever having to configure anything on AWS.

(We _could_ do everything that Heroku does for us on our own on AWS, but that increases
complexity, and opens you up to forgetting to turn something "off", and you'd start incurring
$$ costs).

There are various ways to interact with Heroku, but I'm going to have you do the methods that I'm
most familiar with.

## 1. Create a heroku account

[Create a heroku account](https://signup.heroku.com/dc) if you don't already have one.

## 2. Complete parts of the heroku getting-started-with-python tutorial

Go through the [heroku getting-started-with-python tutorial](https://devcenter.heroku.com/articles/getting-started-with-python).

In this tutorial, you will:
* install the heroku cli.
* clone an example git heroku-ready repository and deploy it to heroku
* provision a postgresql database and connect it to your app
* work with a `requirements.txt` file (you've seen this before! and a `Procfile` (a heroku-specific thing)

## 3. Deploy your own flask app to Heroku

Using what you learned from the tutorial, get your flask app running on heroku.

Some tips:

### Tip 1: Use a production-ready web server

You _could_ run your flask app on heroku via the `flask run` command, like you do locally, but
[the flask server is not recommended for a production environment](https://devcenter.heroku.com/articles/python-gunicorn). If you decide to straight-flask anyway
as a learning experience, then you need to tell flask to run on `$PORT`. Your app's "web dyno" will
get a `PORT` env var set by heroku, and your web process must bind to this port in order to receive
traffic via your heroku domain name. You also must listen on `0.0.0.0`, I think. By default, flask runs on
host=127.0.0.1, and port=5000. You need to bind to 0.0.0.0:$PORT because your app gets traffic
reverse-proxied to it from something like nginx that heroku has sitting in front and receiving
traffic on typical web ports 80 (for http) and 443 (for https).

If you decide to follow the tutorial's suggestion and use `gunicorn` in your Procfile, you need not worry.
[gunicorn binds to `0.0.0.0:$PORT` by default](https://docs.gunicorn.org/en/stable/settings.html#bind),
if the `PORT` environment variable is defined.

### Tip 2: Run locally to do fast development cycles

If you run your app locally using `heroku local`, your development cycle can be much faster than doing the
git-commit-push-wait-for-build-run-heroku-logs flow.

To see errors on your deployed app, use the `heroku logs` cli comment.

### Tip 3: You have two different `git remote`s now!

When you run `heroku create` from your project directory, heroku adds a `heroku`
git remote to your `.git/config` file. (See it by running `git remote -v`).
So now, when you want to push to heroku, you need to specify `git push heroku`.
When you want to push to your github remote, assuming it is called "origin", do
`git push origin`.

## Deliverable

Using the same repo that you've been using for the previous labs, do the following:
* Deploy your app from the previous labs to heroku.
* Create a new file called `test_heroku.py` that looks just like `test_flask.py`
  does, except make it use your heroku domain now.
* Add a new section to your `README.md` called `Deployed model`, and describe something like how the model is hosted on heroku, and how
  someone who wants to use your model can point at the heroku domain. Specify your heroku domain predict route in your readme.

Then, submit to canvas a link to your github repo.
