---
title: Lab -- Deploying an sklearn model to GCP Cloud Run
order: 99
description: >
  In this lab, you will deploy a Flask API to GCP serving a sklearn pipeline, using
  Github Actions for continuous integration / continuous delivery (CI/CD), which will redeploy the app on code-push.
published: true
---

We'll do this lab assuming you already have a cloudpickle'd pipeline ready to go,
that you'd like to serve from a Flask app.

This lab assumes you already have a GCP account.

We'll avoid setting up the `gcloud` cli locally, just to reduce friction.

We'll make a Docker container that runs `gunicorn` to serve our flask app.

But first, let's start from this GCP quickstart tutorial: [Deploy a Python service to Cloud Run](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-python-service).

# Create a GitHub repo

* Create a new github repository and clone it to your local machine. I called mine `cloud-run-test`
* In your new repo, follow all the steps in the section [Writing the sample application][gcp-cloud-run-python-quickstart]. This will give you a directory structure like this:

  [gcp-cloud-run-python-quickstart]: https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-python-service#writing

  ```
  cloud-run-test
  ├── Dockerfile
  ├── README.md
  ├── docker-compose.yml
  └── requirements.txt
  ```

* Push your code to GitHub

# Create a GCP Project and Service Account

Create a project on GCP that will house this deployment. I called mine `cloud-run-hello-world`

Now, we have two options. We are going to host our source code on GitHub. We can either give GCP permission to access our GitHub account, or we can go the other way around.

# Option 1: Authorize GCP to access our GitHub account.

With this, GCP will have permission to "Act on your Behalf" on GitHub.

That's too permissive for my liking. If it were possible to limit
GCP's scope to just one repository, I might be more comfortable with it.

But FYI, it would look like this:

{% include lab-image.html image='lab-cloud-deploy-gcp-grant-github-1.png' %}
{% include lab-image.html image='lab-cloud-deploy-gcp-grant-github-2.png' %}

# Option 2: Authorize GitHub to access our GCP account.

We set "Secrets" (environment variables) that are used by GitHub Actions, that get
triggered on GitHub events, such as a code push.

I prefer this option. We can create fine-tuned GCP "Service Accounts" that only
have permission to operate on specific projects.

Let's do this option.

## Create a new Github Action workflow file

Github Action has templates -- let's find one for "Cloud Run".


{% include lab-image.html image='lab-cloud-deploy-github-actions-search-cloud-run.png' %}

I choose the second one.


## Configuring the workflow

Read its documentation -- the commentblock at the beginning of the file.

{% include lab-image.html image='lab-cloud-deploy-workflow-documentation.png' caption='The documentation for this workflow is in the commentblock at the top of the file.' %}

It has five steps.

### Step 1: "Ensure the required Google Cloud APIs are enabled:"

It says we need to enable:
* Cloud Run
* Cloud Build
* Artifact Registry

Use GCP Console's API Services page and its API Library page to make sure
each of these services is enabled.

{% include lab-image.html image='lab-cloud-deploy-gcp-find-api.png' caption='Finding an API' %}
{% include lab-image.html image='lab-cloud-deploy-gcp-enable-api.png' caption='Enabling the Cloud Run API' %}

If you mess up, you will get a helpful specific
error message when you try to build telling you which API you need to enable.

{% include lab-image.html image='lab-cloud-deploy-api-enable.png' caption='An error telling you to enable an API' %}


### Step 2: "Create and configure Workload Identity Federation."

Follow the link to <https://github.com/google-github-actions/auth#setting-up-workload-identity-federation>. This page describes `google-github-actions/auth`, which this Github Action template uses. It says that setting up a Workload Identity Federation would enable GitHub to send requests impersonating a GCP Service Account *without needing to authenticate as the service account*. Instead, it authenticates as GitHub, which we grant permission to impersonate the Service Account.

Now that looks super cool, but I'm more familiar with Service Account Key JSON (also described
on that page), so we'll do that.

Use GCP Console IAM to create a new "service account". Note its email address.

{% include lab-image.html image='lab-cloud-deploy-create-service-account.png' caption='Creating a service account.' %}

Download a `json` "keyfile" for this service account. This is how we'll authenticate from
github as this user.

{% include lab-image.html image='lab-cloud-deploy-gcp-save-private-key-0.png' caption='' %}
{% include lab-image.html image='lab-cloud-deploy-gcp-save-private-key.png' caption='' %}


Scroll down in the workflow file and comment out the first Google Auth block, and uncomment the second one:

```yaml
#       - name: Google Auth
#         id: auth
#         uses: 'google-github-actions/auth@v0'
#         with:
#           workload_identity_provider: '${{ secrets.WIF_PROVIDER }}' # e.g. - projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider
#           service_account: '${{ secrets.WIF_SERVICE_ACCOUNT }}' # e.g. - my-service-account@my-project.iam.gserviceaccount.com

      # NOTE: Alternative option - authentication via credentials json
      - name: Google Auth
        id: auth
        uses: 'google-github-actions/auth@v0'
        with:
          credentials_json: '${{ secrets.GCP_CREDENTIALS }}'
```


But right now, even though we can *authenticate* as this user, this user isn't authorized to *do anything*. Let's fix that.

### Step 3: Ensure the required IAM permissions are granted

Give our service account the permissions that the workflow docs say it needs:

* Cloud Run Admin
* Service Account User
* Cloud Build Editor
* Storage Object Admin
* Artifact Registry Admin

To do this:
1.  Visit the "IAM" page for your project.
    {% include lab-image.html image='lab-cloud-deploy-gcp-permissions-page.png' caption='' %}

1.  Click to add a "principal." Begin to type your service account's email address -- it should suggest an auto-complete. Choose this.

    {% include lab-image.html image='lab-cloud-deploy-gcp-add-principal.png' caption='' %}
1.  Add all of the "Role"s shown below:  
    {% include lab-image.html image='lab-cloud-deploy-gcp-permissions-0.png' caption='' %}

### Step 4: Create GitHub secrets

GitHub "Secrets" are a way to pass sensitive values to your workflows via environment variables.

Look at the Google Auth entry in the YAML again. Note that it templates {% raw %}`${{ secrets.GCP_CREDENTIALS }}`{% endraw %}. We need to set this as a "Secret" in our github repository.
It should be the text value of the json keyfile you downloaded earlier, when you
created the service account.

Visit your repository's "Secrets" page, and click to add a new secret:

{% include lab-image.html image='lab-cloud-deploy-gcp-secrets.png' caption='' %}

{% include lab-image.html image='lab-cloud-deploy-gcp-secret-credentials.png' caption='' %}


### Step 5: Change the values for the SERVICE and REGION environment variables (below).

Scroll to around lines 54-57 in your workflow file. You will see three env vars that must
be updated:

{% include lab-image.html image='lab-cloud-deploy-github-env-vars.png' caption='' %}


We need to set `PROJECT_ID`, `SERVICE`, and `REGION` under the `env` key.
- `PROJECT_ID` has to match your GCP project's id. You can find it from your GCP console
  project selector:
  {% include lab-image.html image='lab-cloud-deploy-gcp-project-id.png' caption='' %}
- `SERVICE` can be any text value.
- `REGION` has to be from the list shown when you run `gcloud artifacts locations list` from a gcloud shell. I arbitrarily chose `us-central1`.

```yaml
env:
  PROJECT_ID: cloud-run-hello-world-347317 # TODO: update Google Cloud project id
  SERVICE: my-cloud-run-service-yay # TODO: update Cloud Run service name
  REGION: us-central1 # TODO: update Cloud Run service region
```

## Commit your workflow, and watch the build

Commit your workflow file.

{% include lab-image.html image='lab-cloud-deploy-github-save-workflow.png' caption='' %}

This will create a new file in your repo in a new `.github/workflows` folder.
The creation of this file, in turn, triggers your workflow to run, since I think because we configured it at the top to run on any `push`.

Watch it run:

{% include lab-image.html image='lab-cloud-deploy-cloud-run-test.png' caption='' %}

## An error!

Oh no, it threw an error!

{% include lab-image.html image='lab-cloud-deploy-need-storage-permission.png' caption='' %}

So I go back to the GCP cloud console and take a guess and add another permission: "Storage Admin". Now my permissions list looks like this:

{% include lab-image.html image='lab-cloud-deploy-gcp-permissions.png' caption='' %}


I rerun my action.

{% include lab-image.html image='lab-cloud-deploy-github-rerun-failed.png' caption='' %}

It worked this time!

## Attempt to access the service's web url

Part of the build process was to print out your service's endpoint url.

{% include lab-image.html image='lab-cloud-deploy-gcp-url.png' caption='' %}

Access it in a browser:

{% include lab-image.html image='lab-cloud-deploy-gcp-cloud-run-forbidden.png' caption='' %}

Oh no, access denied!

Do some google searching, and eventually come across [this note](https://github.com/google-github-actions/deploy-cloudrun#allow-unauthenticated-requests):

> A Cloud Run product recommendation is that CI/CD systems not set or change settings for allowing unauthenticated invocations. New deployments are automatically private services, while deploying a revision of a public (unauthenticated) service will preserve the IAM setting of public (unauthenticated). For more information, see Controlling access on an individual service.

## Grant public invoker access and try again
Okay, so let's manually enable unauthenticated requests. We have to give "invoker" rights to "allUsers":

{% include lab-image.html image='lab-cloud-deploy-gcp-cloud-invoker.png' caption='' %}

{% include lab-image.html image='minecraft-evoker.png' caption='Note: This is an "evoker", not an "invoker"' %}

Retry your web url:

{% include lab-image.html image='lab-cloud-deploy-gcp-cloud-run-hello-world.png' caption='' %}

Phew, we did it! Holy smokes we did it! We have CI/CD set up for an amazing hello-world!


# Deliverable: Modify your repo to serve a ML model

Now, we can develop our Docker container gunicorn app locally, and then git-push our changes to our github repo, which will trigger a redeploy of our Cloud Run service. Hot dog!

Your goal is to write a Flask API with a `/predict` route that accepts posted JSON.

## Tips

### Develop locally, then deploy when ready!

Use skills you have learned in previous labs to develop your app locally. Only once it's
working locally should you begin to deploy it to Cloud Run.

**Remember you have Different Environments**

Your app will run in two different environments:

1. When you are developing locally and use `docker-compose up`, you'll use Flask's development server
1. When your container runs on GCP Cloud Run via just `docker run`, it will use `gunicorn`'s server.

This means that locally, flask-specific environment variables will be respected, but
they will have no effect when run on GCP. This is good! That's exactly what the flask
built-in development server is intended for.

## Required files

Make a <span class='badge badge-success'>new github repository</span> that contains <span class='badge badge-danger'>only</span> the following files.
<span class='badge badge-info'>Submit a link</span> to this repository.

The files should include:
```
cloud-deploy
├── .dockerignore
├── .gitignore
├── .github
|   └── workflows
|       └── google-cloudrun-source.yml
├── docker-compose.yml
├── Dockerfile
├── main.py
├── phish-model-1649995335.cloudpickle
├── README.md
├── requirements.txt
└── make-request.py
```

## Use this model!

Use the following model in your app. You can commit the file to your repository.

[phish-model-1649995335.cloudpickle](https://storage.googleapis.com/security-analytics-models-public/phish-model-1649995335.cloudpickle)

**Specify in your README where this model came from**

This is an sklearn pipleline. It was fitted using a completed version of [this notebook](https://github.com/deargle-classes/security-analytics-assignments/blob/main/notebooks/lab-pipelines-template.ipynb), and hence:
* it should be unpickled using the `cloudpickle` package
* it was trained on
  [PhishStorm](https://ieeexplore.ieee.org/abstract/document/6975177) data
* it uses a
  [`svm.SVC`](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)
  model as its classifier, and can do `.predict()` (it can't do
  `.predict_proba()`)
* it expects to be given a pandas df with one column called `domain`
* each url is expected to already have the protocol stripped.
  * For example, `example.com`, not `http://example.com`
* each url can also include the path and any query
  * For example, `example.com/subpath/yeet` is valid.
* each _domain_ portion of the url must end with a `/`, even if the url does not include a path
  * For example, `example.com/`, not `example.com`


## README.md

Minimum contents:
- [ ] an overall description of the repository
  - Should describe how this is an API, and what it is used for
- [ ] **The public url that can be used to consume your Cloud Run Services API**
- [ ] a `docker-compose` command to run your Flask container  
- [ ] Should have a usage example:
    - [ ] a `docker-compose` command to execute `make-request.py` within a running container.

## Dockerfile

You can start from the Dockerfile that is in the [GCP Cloud Run python example][gcp-cloud-run-python-quickstart] -- specifically, [this file](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/run/helloworld/Dockerfile).

**Attention!**

But you must modify the python version to be the same as what the model was trained with: `python:3.8`
  - [ ] You can use `python:3.8-slim`

## docker-compose.yml

This file will only be used for local development!

- [ ] It should invoke `flask run` instead of the Dockerfile's default `CMD` of `gunicorn`.
      - Hint: use [the `command` key](https://docs.docker.com/compose/compose-file/#command)
        to override the Dockerfile's `gunicorn` `CMD`.

## .gitignore

- [ ] It should exclude stuff that needs to be excluded.

## requirements.txt

- [ ] Use this `requirements.txt` file:

  ```requirements
  Flask==2.0.31
  requests==2.27.1
  scikit-learn==1.0.2
  cloudpickle==2.0.0
  pandas==1.4.0
  gunicorn==20.1.0
  ```

## main.py

This is your Flask app.

<div class='alert alert-warning'><strong>Gotcha!</strong>Note that we are using <code>main.py</code> for our Flask app, not <code>app.py</code>. This is because the example
Dockerfile <code>CMD</code> specified <code>main:app</code> and not <code>app:app</code>., and we are lazy, but in a good way.</div>

### Config

- [ ] Run the Flask app *without* the debugger but *with* the reloader.
  - Do this in your docker-compose file, don't set Flask envs in your Dockerfile!
  - The following environment variables in your docker-compose should work:

    ```yml
    environment:
      FLASK_ENV: development
      FLASK_DEBUG: 1
      FLASK_RUN_RELOAD: 1
      FLASK_RUN_DEBUGGER: 0
      FLASK_APP: main.py
      FLASK_RUN_HOST: 0.0.0.0
    ```

- [ ] As in [the flask docker json api lab]({% link _labs/lab-flask-docker-json-api.md %}),
      exceptions inside Flask routes should be returned as JSON, with the stacktrace printed to stderr.

### Routes

Your flask app should have the following routes, **all of which must return JSON**:
- [ ] A `POST` route named `/predict`
  - expects a `list` of URLs
  - **Attention!** There are two data-preparation annoyances that should have been dealt with at pipeline-creation
  time, but alas, you have to deal with them here, in this route.
    - [ ] Your route must take care of removing **any** protocol in the url
      - e.g., it must remove things like `http://`, `https://`, `ftp://`, and more.
      - Hint: use regex
    - [ ] The pipeline expects every URL to have at least one `/` in it, after the domain. Ensure that it does!
      - e.g., `yeet.com` would need to be replaced with `yeet.com/`.
  - [ ] returns predictions for whether each URL is a phish, using the following format:

    ```python
    {
      timestamp: TIMESTAMP,
      predictions: [
        {
          url: URL_1,
          prediction: PREDICTION
        },
        {
          url: URL_2,
          prediction: PREDICTION
        },
        # ... one for each posted URL
        {
          url: URL_n,
          prediction: PREDICTION
        },
      ]
    }
    ```


- [ ] **All routes should have a docstring**. See
      [pep-257](https://peps.python.org/pep-0257/). You may write any docstring, as
      long as it's somehow route-relevant.

## make-request.py

This file should use the [`requests`](https://docs.python-requests.org/en/latest/) library to
hit your flask app `predict` route.

Complete the template below.


```python
# ~~~ make-request.py ~~~

urls = [ # all stolen from https://phishingquiz.withgoogle.com/ on 2022-04-27
  'https://drive--google.com/luke.johnson',
  'https://efax.hosting.com.mailru382.co/efaxdelivery/2017Dk4h325RE3',
  'https://drive.google.com.download-photo.sytez.net/AONh1e0hVP',
  'https://www.dropbox.com/buy',
  'westmountdayschool.org',
  'https://myaccount.google.com-securitysettingpage.ml-security.org/signonoptions/',
  'https://google.com/amp/tinyurl.com/y7u8ewlr',
  'www.tripit.com/uhp/userAgreement'
]

def do_request(urls):
  #    1. Make the request
  #    1. Check the request for errors; handle (print) errors if so
  #    1. Assuming no errors, print the predicted response
  pass

# 1. First, `post` all the urls at the same time
do_request(urls)

# 2. Then, loop over the urls and post one at a time.
for url in urls:
  do_request([url])

```


## Github Action Workflow

- [ ] You should write a Github Actions workflow that deploys your repo to Cloud Run on push.

## Recommended Path to Completion

This is a big deliverable! Here's a recommendation for how to go about completing it. Test that things are working at each step.

1.  Get the Hello World GCP Cloud Run working
1.  Get your local flask docker-compose dev environment working.
    - With live-reloading
    - with the development server
    - Without the debugger
    - With errors caught and printed
1.  Write a `/predict` route that loads your model, and nothing else
1.  Modify that route to feed a static url into the model,
and return a prediction
1.  Modify that route to get a list of urls from a POST as json, and feed them into the model.
    - Handpick the URLs to be ones that don't have the protocol and `/` problems
1.  Modify the return value format to match the expected format
1.  Modify the route to handle the `://` and `/` problems
1.  Deploy your route to Cloud Run
1.  ??? Profit
