---
title: Lab -- Developing apps in python
learning_objectives:
  - Get python, pip and the like
  - Use python virtual environments
order: 6
published: false
---

We're working up towards you deploying a python app on heroku, a Flask app, that
will use a REST api, receiving data for which a prediction is desired,
running it against a pipeline and pre-fit model, and returning the prediction in
json format. First things first! You need a sane python development environment. This
is hard, since we all have different computer setups. Once this all is done though,
we should have more-or-less kind-of-identical or identical-enough python development
environments.

You've used `docker` before -- `docker` gives you isolated, reproducible
entire operating system environments so that apps can run precisely replicably
without worrying about interactions with any other apps running on a system.
virtual environments are like docker, except not. They're ways to isolate
_python apps_ from one another -- ways to bundle python installed packages.
virtual environments give you the ability to bundle a `requirements.txt` file
with your python app that says exactly which python packages -- and package-versions --
need to be installed in order for the app to work. You ship the requirements file
with your app, not the actual packages.

In this lab, you'll get a way to use `python` and `pip` within a virtual environment.

* If you're using Anaconda python, you'll install `pip` from the anaconda repository into a `conda env`, and then you'll use
`pip` from then on out.
* If you're not using Anaconda python, you'll use
`virtualenv` (via `virtualenvwrapper`). **Virtualenvwrapper does not work in anaconda python**.

The deliverable is to create a mostly-empty github repository that we'll use
for future labs.

<div class='alert alert-danger'><strong>Heads up!</strong> If you're using <strong>Anaconda python</strong> on either Windows or Mac, then you must use anaconda's
built-in <code>conda env</code> as your virtual environment manager. You <em>cannot</em> use
<code>virtualenv</code>.</div>


## If you're using Windows, get a Linux environment

* **If you're using Windows**, you should learn how to use
  Windows Subsystem for Linux (WSL) to install a linux distro, and you should
  learn how to do python development within that distro. It will make some development things
  easier. I don't have proof, just feelings. Do this even if you already have
  python installed within Windows-proper.

  Once you have your WSL Ubuntu set up, I recommend figuring this out _without_
  using Anaconda python.

* **If you're using Mac**, you already have a Linux-y-enough environment.

## If you're using Anaconda, skip down to...

If you're using Anaconda, then skip all the way down to [this section](#making-a-virtual-environment-using-conda-env--for-anaconda-users).

## Windows Users -- Get Python and Pip within WSL Ubuntu

The following describes getting python and pip working within WSL Ubuntu. You
must set this up regardless of whether you have installed python within your
Windows base install.

### Get WSL

Check whether you already have it:

    wsl --help

If you don't already have it, [get WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10),

### Install the Ubuntu distribution

Check whether you already have it:

    wsl --list

If you don't, then [install the Ubuntu distribution](https://ubuntu.com/wsl)

### Set the Ubuntu distribution as your default

See what your default wsl distribution is:

    wsl --list

If it's not already, [set the Ubuntu distribution as your default](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#set-a-default-distribution)

### Launch into WSL

One way is by opening a windows command prompt and running the `wsl` command:

    wsl

This will launch your default wsl distribution, in the current windows directory.

### Get Python in ubuntu

You might already have it. Try running the following:

    which python

If that showed that the `python` command is available, then **skip the rest of the section**.

But if that didn't work, then see whether you have python3:

    which python3

If you have the latter but not the former, then your python dev life will be a lot easier
if you fix it so that running `python` runs some kind of python executable.

#### Fix it via apt

The easiest way to fix this according to a blog post I read somewhere is to install
the `python-pip` package. The downside of this is that it gives you the old-and-unsupported
`python2` and `pip2`. In fact, don't do this, it will cause headaches later on, forget it.

#### Fix it via symbolic linking

This way is cool. If you create a symbolic link (symlink) to a python executable that will run as `python`.
In linux, the `ln` command can create symbolic links. I always forget the command
argument order, therefore you will do. So check it first:

    ln --help

Mine reminds me that the first usage form is this:

    Usage: ln [OPTION]... [-T] TARGET LINK_NAME

`TARGET` has to point to a program that actually exists. `LINK_NAME` will be the name of a
sort of shortcut that will actually just run `TARGET` under the hood.

So imagine that for me, `which python` returned nothing, but `which python3` returned
`/usr/bin/python3`. I would create the following symlink to make it so that I can run
`python` to actually run `/usr/bin/python3`:

    ln -s /usr/bin/python3 /usr/bin/python

I'm putting it in the `/usr/bin/` directory because that's on my `$PATH`, ask me more
if that's confusing.

Now when I run `which python`, it tells me `/usr/bin/python`. This means that I have
the `python` command available to run python.

### Get Pip in ubuntu

Pip is a python package manager. See if you already have it:

    which pip

If you do, **skip the rest of this section.**

#### I don't have pip

If you don't have it, see if you have `pip3`:

    which pip3

If you do, follow the same steps as above to symlink `/usr/bin/pip` to `/usr/bin/pip3`.
Make sure it's working by checking `which pip` or `pip -V`.

#### I don't have pip3 either

You poor sap. Okay, install it via `apt`. First, update `apt`'s listing of what packages
are available where (this doesn't install anything, it just updates the directory listing):

    sudo apt update

Then, install pip3:

    sudo apt install python3-pip

This will give you the `pip3` command. Then, symlink `/usr/bin/pip` to `/usr/bin/pip3`
for great justice.

## Mac users -- Get `python` and `pip`

Mac users: **Assuming you're not using anaconda python**: [follow this guide to get python and pip, it looks easy enough](https://docs.python-guide.org/starting/install3/osx/).
Report back? Sorry, I'm not a Mac user.

If you're using Anaconda, you already have access to `python` and `pip`.

## Creating a virtual environment

Now you need virtual environment to make your soon-to-be-dozens
of python apps in. Dozens because you'll get so in to this that you won't be
able to stop. If you're not using anaconda, follow the section below for "virtualenvwrapper."
If you're using Anaconda, skip below to the section "Using conda env for Anaconda users."

### Non-Anaconda users -- Using Virtualenvwrapper

I use a python package called [`virtualenvwrapper`](https://virtualenvwrapper.readthedocs.io/en/latest/)
which ... wraps... `virtualenv`. I recommend you use it too. Install it, using `pip`,
like this:

    pip install virtualenvwrapper

It will install a file called `virtualenvwrapper.sh` somewhere in your ubuntu `$PATH`.
You need to find it, because we need that shell script to be run every time you
launch a new wsl ubuntu shell. Running that script will make the package's
wrapper functions available in your shell environment.

It either gets installed into _~/.local/bin/_, here:

    ~/.local/bin/virtualenvwrapper.sh

Or, it gets installed into _/usr/local/bin_, here:

    /usr/local/bin/virtualenvwrapper.sh

Check both, first one first.

Once you've found it, you need to add a line to the bottom of one of your shell
startup files -- I add mine to `~/.bashrc` . Edit that file using vim, nano, whatever.
Assuming your virtualenvwrapper.sh file is located where mine is, you would add the following
line to the bottom of your `~/.bashrc`:

    source ~/.local/bin/virtualenvwrapper.sh

The `source` command runs the file.

#### Make a virtual environment using virtualenvwrapper

Now, moment of truth -- try making a virtualenv! Adapting from the project's
[quickstart](https://virtualenvwrapper.readthedocs.io/en/latest/install.html#quick-start),
let's make one called 'yay-virtual':

~~~bash
$ mkvirtualenv yay-virtual
created virtual environment CPython2.7.17.final.0-64 in 8271ms
... [snipped] ...
(yay-virtual) $
~~~

This creates and _activates_ a virtual environment called 'yay-virtual'. To the left
of your shell prompt, you should see `(yay-virtual)` -- this tells you which environment
you have active.

#### Using your virtualenv virtual environment

This environment contains its own `python` and `pip` executable copies. Any
package you install using `pip` while in this environment will go into a environment-specific
directory, isolated from all other environments' packages.

Install a package: for instance, install `pandas`:

    pip install pandas

Start a python interactive environment and confirm that _pandas_ is available:

~~~bash
$ python
>>> import pandas as pd
>>>
~~~

This should have run without error, as indicated by the absence of an error message
after the import statement. Exit the interactive python session via `ctrl + d`.

Now, "deactivate" the virtual environment:

    deactivate

The `(yay-virtual)` disappears from the left of the prompt. Any packages that
were installed in that environment, that have not also been installed globally,
are now not available. Confirm:

    $ python
    >>> import pandas as pd
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    ImportError: No module named pandas
    >>>

Get back into your environment at any time by running `workon`:

    workon

Running the command with no arguments will output a list of known available virtual environments.

Run the command with the name of an environment to activate that environment:

    workon yay-virtual

And you're back in!

#### Optional -- associate a directory with that environment

As your number of python projects grows exponentially, so will your number of
environments. It's good to have one environment per project. It's also good to have
one directory per project. So, tie the two together.

_virtualenvwrapper_ provides a convenience function called `setvirtualenvproject`.
If you execute that command with no arguments, whatever directory you execute that
command in will become the currently-active environment's _project directory_.
Now, if you drift away to another directory in your terminal, you can jump back
by running `cdproject`. Also, whenever you activate that project in the future,
your terminal should automatically change directory to the project's directory.
It's cool!

#### Deleting unwanted virtual environments

_virtualenvwrapper_ provides a `rmvirtualenv` command which will take care of
virtualenv cleanup for you. If you decide you're done forever with 'yay-virtual',
you could run the following:

    rmvirtualenv yay-virtual

Following this, running `workon` should no longer show 'yay-virtual' as an option.

### Anaconda Users -- Make a virtual environment using `conda env`

The process is considerably simpler because anaconda comes with `conda env` ready to go.
The steps below are from [the official anaconda docs for managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html). I'm assuming that you're using conda >= v4.6.

**Pay attention to this:** conda has its own [package repository](https://anaconda.org/anaconda/repo)
that is entirely separate from [pypi](https://pypi.org/). I don't use conda packages -- I use pip packages.
Pip packages work in both python and anaconda, but surprise, conda packages only work in conda.
That's why I think you should develop with pip packages only.

So! The instructions below show you how to create a conda environment with the `pip` _conda_ package installed.
This is correct -- install pip into your conda environment using the conda package manager. But after that,
_stop using the `conda install` within your environment_. Instead, use `pip install` etc. As soon as you use
`pip` within a conda environment, conda no longer "knows" about the state of the environment.

Do the following three things:

* [Create an environment with pip](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#using-pip-in-an-environment)

* [Activating an environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#activating-an-environment)

* [Deactivating an environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#deactivating-an-environment)

## Using pip to install everything in a file

This section works for both `virtualenv` and `conda env`.

You can use pip to install all packages listed in a file by running
`pip -r <filename>`. This is used often when you deploy your project to other environments
-- say, to cloud-hosted Heroku. Heroku will recognize that your app is a python app,
and then look for a file called _requirements.txt_, and if found, automatically install
everything in there using `pip -r requirements.txt`. And if you share your repository
on github, anyone can clone it, create a new virtual environment for it, then run
`pip -r requirements.txt` on their clone and get all of the packages they need.

Side note, if you are making an installable python package, then your install instructions
(your `setup.py` file) can optionally read your requirements.txt and instruct the install process to install everything
listed in there. This is common.

This all assumes, of course, that you keep your requirements.txt up to date with all of the packages
that are actually needed, along with their precise versions! Fight dependency hell!

## Flow for actually using virtual environments

* Create one virtual environment for each of your python projects
* When switching between projects, deactivate/activate the associated project environment.
* Do _not_ commit environment files to your git repository.
* Only `pip install` with an environment active.
* Keep your pip dependencies up-to-date, with version numbers, in a file called
  `requirements.txt` in the root of your project directory.
  Commit this file to your git repository.
* Add a "quickstart" line to your project's README.md for how to get the code (`git clone <repo url>`),
  how install all dependencies (generic "into a virtual environment, run `pip install -r requirements.txt`"),
  and how to "use" your project (will vary!).
* If your environment gets borked, blast it away and make a new one.

## Deliverable

This whole lab is preparation for future labs. So, do the following:

* Use your virtual environment manager -- whether `virtualenv` or `conda env` -- to create a virtual environment called 'deploy-ml'
* Create a project directory somewhere called 'deploy-ml'
* Initialize your new 'deploy-ml' directory as a git repository (`git init`)
* add a README.md to your directory/project with whatever content,
  at least the title of the project. Commit it (`git add .`, `git commit`).
* create an _empty_ (no optional files added!) github project called, um, 'deploy-ml' I guess.
  Follow the instructions on your new git repository to add a git remote to your local repo
  that points up to github, and push your local repo up to github.
* Submit a screenshot showing both of the following:
  1. A terminal with your "deploy-ml" environment active
  2. Your github repository open in a browser.

# Merge me in later!

This is content I wrote for another lab, but that I'm now taking out. I want to merge it into this lab later.

# Part 2: Install jupyter via pip into a virtual environment

A good practice when developing in python is to work within a separate [virtual
environment](https://docs.python.org/3/library/venv.html) for each projects.
This helps avoid [dependency
hell](https://en.wikipedia.org/wiki/Dependency_hell). It's difficult to describe
how horrible dependency hell can be until you've experienced it yourself, so I
won't try here.

## Make a new folder for your project

First, make a new directory to hold your project. Later, we'll associate this folder
with a project-specific github repo.

Do not nest this folder within another project folder!

## Create a virtual environment

We'll use the `venv` python module for our virtual environments.

We'll follow the [instructions in the venv documentation](https://docs.python.org/3/library/venv.html#creating-virtual-environments).

From your new project directory, run the following:

```bash
python3 -m venv venv
```

* Nowadays, `python3` is better to use for development than python2
* `python3 -m venv` says we want to use `python3` to run the `venv` python "module" (package).
* The second `venv` will be the name of the folder to store our virtual environment


## Activate the virtual environment

Now, find the section [in the
documentation](https://docs.python.org/3/library/venv.html#creating-virtual-environments)
that begins with "Once a virtual environment has been created, it can be
“activated” using a script in the virtual environment’s binary directory.". Read
the instructions there to learn how to activate and deactivate your virtual
environment.

Then, activate your environment.

## Update `pip`

Now that your virtual environment is active, running `python` will point to the executable used to create the python environment (`python3`?)

Also, running `pip` in a virtualenv is equivalent to running `python -m pip`.
This means that you get a brand-new pip. If at any point you upgraded your
system's `pip`, you won't have those updates anymore!

So, make sure your `pip` in your virtualenv is up-to-date:

```bash
pip install --upgrade pip
```

## Install and launch jupyter

Jupyter packages are available on pip. That means we can [install them into our
environment](https://jupyter.org/install). Choose one of the following,
depending on whether you want the (recommended) new hotness, or the old and
classic jupyter server.

After you run these commands, you will have a jupyter notebook environment with
basically zero available packages (no `pandas`, no `scipy`, no `seaborn`...
nothing). But you will have increased knowledge of where jupyter servers come from!

### New hotness

Install:
```bash
pip install jupyterlab
```

Launch:
```bash
jupyter-lab
```

### Old and classic:

Install:
```bash
pip install notebook
```

Launch:
```bash
jupyter notebook
```


## Maintain a `requirements.txt` file

You want others to be able to get the same environment of packages as you have.

Create a file called `requirements.txt` in the root of your project dir.

[Read
more about them in the pip
docs](https://pip.pypa.io/en/stable/user_guide/#requirements-files).

In this file, manually maintain a list of the packages you have installed -- one
package per line. So far, that's either `jupyter-lab` or `notebook`. For now,
don't worry about specifying specific package versions.

So if you had installed `jupyter-lab`, your `requirements.txt` file would now look like this:

```
jupyter-lab
```

If someone else wanted to install all of the packages in this file into their environment, they could simply run:

```bash
pip install -r requirements.txt
```

Note: Several build tools will look for a file called `requirements.txt` in the root of your project, and if found, will fun `pip install -r requirements.txt` so that your application environment has those packages available. These tools include [Heroku](https://devcenter.heroku.com/articles/python-pip#the-basics) and [Mybinder](https://mybinder.readthedocs.io/en/latest/using/config_files.html#requirements-txt-install-a-python-environment).

## Get it all in git

### Create the repo locally and publish it to GitHub

Let's try a new way of creating a repository on github -- by first creating
a git repository on your local drive, and then publishing that repo to github.

Use Github Desktop to turn your project-folder into a git repository. To do
this, you can use the `File` > `New repository...` menu item, pointing it to
your already-existing directory.

You should then have an option to publish your repo to Github. You can do this
even if you don't yet have any commits -- Github Desktop creates one for you.

### Create a `README.md`

Only monsters have repos without READMEs. Make one -- have it start with the name
of your repo. For instance, if my repo were named `FooBar DataScience Notebook`,
I would start with a README.md with a level-1 header like this:

```markdown
# Foobar DataScience Notebook
```

### Create a `.gitignore` file

There are arguments [for and
against](https://stackoverflow.com/questions/6590688/is-it-bad-to-have-my-virtualenv-directory-inside-my-git-repository)
including package dependencies in project git repositories. But the arguments
_for_ aren't relevant for open data science. Which packages get installed via
`pip` depend in part on the operating system and machine architecture. Don't do
it.

All of your package dependencies and binaries get installed into the `venv`
folder. So, add `venv` to a `.gitignore` file in the root of your project. Add
and commit the file to your git repo.

Then, push (sync) to github if you feel like it.
