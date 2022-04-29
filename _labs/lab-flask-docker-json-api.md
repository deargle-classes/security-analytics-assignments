---
title: Lab -- Flask Docker JSON API
description: >
  A hello-world Docker Flask json api app, for noobs.
order: 44
---

Your assignment is to use Docker to create a Flask app that does silly
hello-world kind of things.

This lab document won't do much hand-holding, since we've practiced a lot
with these concepts in class.

**Tutorial Boon alert!**: The [docker python tutorial](https://docs.docker.com/language/python/) has
example code for creating a Flask Docker container, including using
`docker-compose`. But take heed! The deliverable requirements below won't let
you use that tutorial as-is!

# Debugging Flask JSON API routes

Debugging Flask JSON API routes is tricky!

* If flask is run in `development` mode, then a werkzeug html debugger has
  errors return HTML. But HTML returned from JSON API routes is morally wrong.
* Werkzeug "production" mode will print exceptions to `std.err`. This is good,
  but we lose the auto-reloader, which is bad.  
* We can manually enable the reloader in production mode via `flask run
--reload`, but this feels like a dirty hack.
* If we disable `DEBUG`, we get 500-exceptions printed to std.err, but not
  400-errors, which trigger `werkzeug.exceptions.HTTPException` exceptions.

## Solution alert!

Use `development` mode, but disable the `debug` mode, and register a custom HTTPException handler
that logs the exception stacktrace.

Something like this to use development mode without the debugger:

```console
$ export FLASK_ENV=development
$ export FLASK_DEBUG=0
$ flask run
```

The below is a small modification of [this Generic Exception Handler](https://flask.palletsprojects.com/en/2.1.x/errorhandling/#generic-exception-handlers).
* It will always return JSON for exceptions that are raised
  during responses. It also logs the stack trace for these exceptions.
* Exceptions raised outside of responses will print exceptions to std.err.

```python
# Modification of https://flask.palletsprojects.com/en/2.1.x/errorhandling/#generic-exception-handlers
from flask import json
from werkzeug.exceptions import HTTPException
import logging # <-- added

@app.errorhandler(HTTPException)
def handle_exception(e):
    """Return JSON instead of HTML for HTTP errors."""
    # start with the correct headers and status code from the error
    logging.exception(e) # <-- added
    response = e.get_response()
    # replace the body with JSON
    response.data = json.dumps({
        "code": e.code,
        "name": e.name,
        "description": e.description,
    })
    response.content_type = "application/json"
    return response
```

:tada: :tada: :tada:

# Deliverable

Make a <span class='badge badge-success'>new github repository</span> that contains <span class='badge badge-danger'>only</span> the following files.
<span class='badge badge-info'>Submit a link</span> to this repository.

The files should include:
```
flask-docker-hello-world
├── README.md
├── .gitignore
├── Dockerfile
├── requirements.txt
├── docker-compose.yml
├── app.py
└── make-request.py
```




## README.md

Minimum contents:
- [ ] an overall description of the repository
- [ ] a `docker-compose` command to run your Flask container  
- [ ] a `docker-compose` command to execute `make-request.py` within a running container.

## .gitignore

- [ ] It should exclude stuff that needs to be excluded.


## Dockerfile

Minimum contents:
- [ ] `FROM` a [python version `3.9` of your choice](https://hub.docker.com/_/python)
- [ ] `COPY` a local `requirements.txt` and `pip install` it
- [ ] a `CMD` that runs your flask app using some incantation of `flask run`
  - It should bind to all interfaces (i.e., `host=0.0.0.0`)


## requirements.txt

- [ ] Should include all necessary packages, version-pinned.


## docker-compose.yml

Minimum contents:
- [ ] `build` the local (`.`) Dockerfile
- [ ] publish `container` port `5000` to `host` port `5555`
- [ ] mount the current directory into whatever your Dockerfile `WORKDIR` is.


## app.py

This is your Flask app

### Flask app config

- [ ] In some manner, run the Flask app *without* the debugger but *with* the reloader.
- [ ] Exceptions inside Flask routes should be returned as JSON, with the stacktrace printed to stderr.


### Flask app routes

Your flask app should have the following routes, **all of which must return JSON**:
- [ ] A `GET` ping-pong route
  - named `/ping`
  - returns `pong!`
- [ ] A `GET` route that returns the reverse of a random word
  - named `/word`
  - This route should use the python [`requests`](https://docs.python-requests.org/en/latest/) package to fetch a random word from <https://random-word-api.herokuapp.com/word?number=1>
  - It should reverse the word, and then return it
    - For example, if the random word were "yeet", the route should return "teey"
- [ ] A `POST` route counts the length of a string
  - named `/string-count`
  - expects a word to be posted as JSON
  - returns the length of this string

**All routes should have a docstring**. See
[pep-257](https://peps.python.org/pep-0257/). You may write any docstring, as
long as it's somehow route-relevant.


## make-request.py

This file should use the [`requests`](https://docs.python-requests.org/en/latest/) library to
hit each of your flask app routes.

- [ ] complete the three functions below
- [ ] It should check each request's status code, [using either `r.status_code` or `r.raise_for_status()`](https://docs.python-requests.org/en/latest/)

```python
# ~~~ make-request.py ~~~

# route 1 -- ping
def call_ping_route():
  r = ___ # make the request
  return r

# route 2 -- random word
def call_random_word_route():
  r = ___ # make the request
  return r

# route 3 -- string count
def call_string_count():
  r = ___ # make the request
  return r

route_callers = [
  call_ping_route
  call_random_word_route
  call_string_count
  ]

for call_route in route_callers:
  r = call_route()
  ___ # first, check r for errors
  data = r.json()
  print(data) # print the response
```
