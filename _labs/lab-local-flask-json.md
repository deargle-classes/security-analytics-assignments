---
title: Lab -- A Flask app to run model predictions via JSON
order: 8
---

In this lab, you will create a Flask app which will enable users to send
HTTP POST requests with JSON requesting a prediction from a pre-fit ML model.

## Install Flask

[Install Flask](https://flask.palletsprojects.com/en/1.1.x/installation/#installation) into your virtual environment.

## Read the Flask Quickstart

<div class='alert alert-danger'><strong>Important Note for Windows Users!</strong>
  <p>
  Windows users, don't use <code>cmd</code> for <code>flask run</code>! Instead, use powershell. If you use <code>cmd</code>, then your flask app
  will randomly hang on requests until you send the interrupt signal (<code>ctrl+c</code>).
  </p>

  <p>See <a href='https://stackoverflow.com/a/29835010/5917194'>this SO post</a></p>
</div>

Complete the Flask [Quickstart](https://flask.palletsprojects.com/en/1.1.x/quickstart/), through the [Routing section](https://flask.palletsprojects.com/en/1.1.x/quickstart/#routing).

Later sections of this lab will reference later sections of the flask quickstart.

## Using environment variables

In this and future labs, you will use _environment variables_.
Environment variables are key-value pairs that are not explicitly defined in code.
Rather, they exist only in the _environment_ available to your running application.
But they are _accessible_ via code, such as via python's `os.environ`.

When you install Flask via pip, a `flask` command is made available from your shell.
The quickstart tells you that you can configure `flask run` by setting environment
variables such as `FLASK_APP`. The flask docs tell several ways to `export` the
variables. The `export KEY=VALUE` shell command makes the variable available for _any command run for the
remainder of the shell session in which that command was run_. **But!** You do
_not_ have to export variables in order to use them. You can instead just specify them
_in front of each command you run_.

For example, I can either run export once, making it so that I do not have to
set FLASK_APP ever again for that shell session:

```bash
export FLASK_APP=hello.py
flask run
flask run
flask run
```

Or, I can specify it each time I run `flask run`:

```bash
FLASK_APP=hello.py flask run
FLASK_APP=hello.py flask run
FLASK_APP=hello.py flask run
```

The second form may confuse you because the `flask` command is no longer the first
entry in the entered command. But that's allowed. You can set as many KEY=VALUE
pairs as you like before you begin to invoke your command.

Environment variables are useful when you are running your app on different servers:
Consider if you are making an app that connects to a database. Perhaps when you _develop_ locally, your app should use a testing database, but when you
_deploy_ your app, it should use a production database. Rather than hard-coding
different database urls in your code, you can instead write code that reads a
`DATABASE_URL` value from the environment. Then you can set that environment
variable differently on various servers.

This method can also be used to keep from hard-coding access credentials
into code that you deploy to github and the like.

Some python apps such as [python-dotenv](https://pypi.org/project/python-dotenv/)
let you write key-value pairs in files that get read by your code. This lets you
avoid having to `export` any env vars or specify them each time you want to run
a command. `.env` is a common name for such files -- hence the package name.
You typically should _not_ commit any `.env` files
to source code control -- that defeats the purpose! They're used to specify things
that are specific to _where the code is run, or to who runs it_. Things that are consistent regardless
of where the code is run or who runs it should perhaps not be env vars.

Now, [read this section about how flask uses env vars](https://pypi.org/project/python-dotenv/). Install `python-dotenv` into your project, and do the quickstart
again trying some of the ideas. Add notes
to your README addressed to your future self describing how to use your app.






## Make some flask JSON routes

Read the [About Responses](https://flask.palletsprojects.com/en/1.1.x/quickstart/#about-responses) section and the [APIs with JSON section](https://flask.palletsprojects.com/en/1.1.x/quickstart/#apis-with-json) to see how you can return JSON from a flask route.

### Make a GET-method JSON route

Practice by making a route that returns JSON. Baby steps -- first,
write a GET route that returns any JSON you like.

### Test your flask JSON route

First, read the [HTTP Methods](https://flask.palletsprojects.com/en/1.1.x/quickstart/#http-methods) section.

If your flask app is running, you can test its routes using anything that can
make HTTP requests. You have many options.

* You could use a web browser's address bar -- making http requests is a brower's
  primary job! Entering an address in a browser's address bar makes a GET request.
* You could use <https://reqbin.com/> to make a request to your localhost
  flask server. Sites like this let you set custom headers and use other HTTP
  methods such as POST.
* You can use python [`requests`](https://docs.python-requests.org/en/master/)
  package to make the request.

For purposes of this lab, write a new python file called `test_flask.py` that
uses the python `requests` library to make requests against your running
flask app. Add a note to the README about how to use your test script.

### Make a POST-method JSON route

Read the [Accessing Request Data](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request) section to start to get an idea of what the `flask.reqeust`
object is, and how it stores user-sent data. Also read the [The Request Object](https://flask.palletsprojects.com/en/1.1.x/quickstart/#the-request-object). Note the note at the bottom
of that section that points to the full documentation on the [`Requests`](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request) object --
follow the link, and ctrl+f to find the [`get_json`](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request.get_json) method. Read it.

Now, make a flask route called `/predict` that receives data as json and returns a prediction. **Return a list of predictions -- one prediction for each requested prediction.** Some general hints and pointers:

1. Load your testing data into your `test_flask.py` script.
2. use the `.post()` method on the `requests` package, demonstrated in the [requests quickstart](https://docs.python-requests.org/en/master/user/quickstart/#make-a-request). Use the `json=` keyword to
   that method to make it so that the `application/json` content-type header is
   added to your request -- see [this section of the requests quickstart](https://docs.python-requests.org/en/master/user/quickstart/#more-complicated-post-requests) for more details.
3. Load your pickled model in flask. You should load your model at the top
   of your script -- outside of any route. This way, code will run once when the app is
   launched, and not repeatedly whenever a route is requested.
4. Remember from the reading that if you return a `dict` from a flask route,
   then Flask will call its `jsonify()` method on it and return it as a json response. _Not all objects are jsonify-able!_ Predictions returned from sklearn
   models are typically of type `ndarray`. This is not jsonify-able! But regular
   python `list`s are. You can convert a ndarray-type objects to a list using
   [ndarray's tolist()](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.tolist.html)
5. Remember from previous labs that sklearn model `predict` functions expect 2d
   arrays. If the POSTed data is 1d, you can wrap it in a list to make it 2d.
5. Use the requests library's `json()` method on your request's response to get
   a python object representation of the flask route's JSON response.

Make your `test_flask.py` script `print` the returned prediction. Use python
[f-strings](https://realpython.com/python-f-strings/) to print a message that
says: "Good day to you. The prediction for the data is: " followed by the prediction.

## Remember!
- keep your requirements.txt up to date
- keep your README.md up to date (with how to start the flask app, etc)

## Deliverable:

Below is a summary list of what was asked for in this lab. Submit a link to a github repo that implements the below.

* A flask app called `app.py` that has a route called `/predict` that receives
  POSTed JSON data.
  * The route can assume that the POSTed data is in an array, but
    it should be able to handle either 1d or 2d arrays (one requested prediction or many).
  * It should use a pickled model fit against your project data.
  * It can also assume that all data is valid data.
  * It should return a list
    having one prediction (positive class only for classification) per requested prediction.
* A python script called `test_flask.py`
  * It should use the `requests` library
  * It should print "Good day to you. The prediction for the data is: " followed by
    the prediction.


## Next lab:
- we will deploy our flask apps to _the cloud_
