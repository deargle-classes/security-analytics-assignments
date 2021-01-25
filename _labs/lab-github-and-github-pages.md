---
title: Lab -- Github, Markdown, and You
learning_objectives:
  - Create a simple resume using GitHub Pages
  - Write documents using Markdown
  - Demonstrate introductory mastery Git, Github, and Github Pages workflow
    using GitHub Desktop and Atom
order: 2
---

## Create a nice github account

Create an account. Old people say "if you want a job, you must make a good
LinkedIn profile." I say pfft. Make a good github
(or other hosted code repo) profile instead and sk8 or die.

* Choose your username being mindful that it'll be going on resumes, lol
* Set a nice profile picture for yourself.
* Set your name and organizational affiliation

<div class='alert alert-deliverable'>
Deliverable: submit a link to your github profile: e.g., https://github.com/deargle
</div>

Now, complete the [GitHub Hello World project](https://guides.github.com/activities/hello-world/)

<div class='alert alert-deliverable'>
Deliverable: submit a link to your github hello-world repository
</div>

## Read about Git and the GitHub flow

To prepare you for what you are about to do, read the following two guides from the collection of [GitHub guides](https://guides.github.com/):

* [Understanding the GitHub flow](https://guides.github.com/introduction/flow/)
* [Git Handbook](https://guides.github.com/introduction/git-handbook/)

## Create a Github Pages resume webpage.

~~Using [this guide](https://guides.github.com/features/pages/), make a [GitHub Pages](https://pages.github.com/) site.~~

Most of the github-provided pre-baked pages themes are tailored to source code. To make a GitHub Pages portfolio website,
I recommend that you use [@evanca's 'quick-portfolio' theme](https://github.com/evanca/quick-portfolio). To do so,
follow [@evanca's instructions on Medium.](https://blog.usejournal.com/set-up-your-portfolio-website-in-less-than-10-minutes-with-github-pages-d0efa8ff56fd). Following this guide, you could do everything on the github website, but I want you to also learn
how you can make changes to files locally, and then push (upload) your changes. So only follow **Steps 1 through 5** for now.
Later, you will use Atom to complete steps 6 through 9.

**Pay attention:** this theme is a customization of the [jekyll-theme-minimal](https://github.com/pages-themes/minimal) theme.
Follow the link for the minimal theme and look at the README to see how one might customize the minimal
theme. @evanca's theme follows these instructions and makes a few small tweaks to make the site more portfolio-related
as opposed to source code-related. So if you wanted to customize @evanca's theme further, you can follow the instructions
in the jekyll-theme-minimal README.

### Github Desktop

* Install [GitHub Desktop](https://desktop.github.com/)
* Use GitHub Desktop to clone your github pages repository to your local computer

### Atom and Markdown

You need a nice text editor. I like [Atom](https://atom.io/). For this
lab, download, install, and use Atom. After, if you hate it, you can abandon it.

* Download and install Atom
* Use Github Desktop to open your github pages repository in Atom

  ![github-desktop-open-in-editor]({{ 'assets/images/github-desktop-atom.png' | relative_url }})

  **Background information:** Your clone includes a `.git` folder, in which there is a `.git/config` file, which
  GitHub Desktop will populate with information about where the clone _came_ from.
  This makes it possible for git client tools, such as the one bundled with Atom,
  to know _where_ to sync (push) changes to by default. Find and open this file in
  a text editor to see for yourself.

  But in order for Atom's
  bundled git client to know about that folder and config file, the clone folder needs to be
  opened _as a folder_ within Atom, as opposed to you using Atom to open and edit an
  individual file within the repo, were you to manually navigate to the clone folder
  with a file explorer and access the files that way. If you use GitHub Desktop
  to open your clone, this should be automatic.

  Git repositories typically only contain one `.git` folder, at the root of the
  repository. IDEs such as Atom therefore look to the root of the currently opened
  folder for a `.git` folder.

* Hook Atom up to GitHub ([the package comes bundled with Atom, because All Hail Octocat](https://github.atom.io/)).

  Click on the Github tab towards the bottom-right of your Atom window. Complete its
  login form.

  ![atom-github-tab]({{ 'assets/images/atom-github-tab.png' | relative_url }})

Next, customize your `index.md` using the Markdown language, and then push your
local changes to your github-hosted GitHub Pages repository.

* Learn about Markdown. Specifically, read about [GitHub-flavored Markdown](https://guides.github.com/features/mastering-markdown/).

* Using Atom instead of the GitHub website proper, follow the instructions in
  [@evanca's instructions on Medium.](https://blog.usejournal.com/set-up-your-portfolio-website-in-less-than-10-minutes-with-github-pages-d0efa8ff56fd) **steps 6 through 9**. In these steps, you will do the following:

  - upload a profile picture

    #### Getting the image file into your repository

    You need to copy your image file into your repository, using something like file explorer. You can quickly navigate
    to where your repo is stored on disk by using the "View the files of your repository in \[Explorer\|Finder\]" menu option in
    GitHub Desktop. Commit the image file to your repository.

    #### Setting the image url

    Also, github has a special url format that will let you fetch a _raw file_ as opposed to a _view of the file wrapped in the GitHub UI_.
    To get the raw file, append `?raw=true` to the url.

    See for yourself. Visit the forked repository's
    logo file _without_ using `?raw=true` here: <https://github.com/evanca/quick-portfolio/blob/master/images/demo.gif>. Note
    that it is wrapped in the GitHub UI. Then, visit it _with_ `?raw=true`: <https://github.com/evanca/quick-portfolio/blob/master/images/demo.gif?raw=true>. You get the raw image file. This is what you need, so that your logo can be
    displayed in your website.

    Note that the forked repository does this:

        # in _config.yml
        logo: "/images/logo.png?raw=true"

    Even though the log file stored in their repository's `/images/` folder is named just `logo.png`.
  - customize your `_config.yml`
  - customize your `index.md`

* Using Atom, commit and push your changes. Check that your website has updated! It might take a minute. You should
  get an email from github if you broke anything with your `_config.yml` edits.

<div class='alert alert-deliverable'>
Deliverable: A link to your github pages website (e.g., https://deargle.github.io), and also to its backing repository (e.g., https://github.com/deargle/deargle.github.io)
</div>

<div class='alert alert-deliverable'>
Deliverable: Screenshot committing and pushing your changes, demonstrating local git workflow.
</div>

Warning! Web dev is a sinkhole activity! You can spend _days_ on this, at the
expense of your other work. Just stick to getting the basics up, then _get out_.

## Supplemental

### Using a custom domain for your `<username>.github.io` site

My website, <https://daveeargle.com>, actually points to a GitHub Pages site at
<https://github.com/deargle/deargle.github.io>. If you want a swanky domain name too,
then you can [follow these instructions](https://docs.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site). The general idea is:

1. Purchase a domain name from a domain name
   registrar such as [Cloudflare](https://cloudflare.com).

2. Add a CNAME DNS record for your domain, setting your domain name to be a CNAME
   for your github-provided `<username>.github.io` domain.

   By default, your domain name registrar will also provide DNS service for you.

3. On the Settings tab for your github repository, add your purchased domain name
   to the "Custom domain" field, and save.

   This will add a file called `CNAME` to the root of your repository, with your
   domain name as its contents.

3. ??? Profit.

### How can I test my changes locally without pushing live?

Read [this github guide](https://docs.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll).
The guide will instruct you to install Ruby and Bundler. If you're on windows, one of the coolest ways to get Ruby is to [install a linux distro such as Ubuntu using Windows Subsystem for Linux (WSL)](https://ubuntu.com/wsl). Read below for some hand-holding tips.

General one-time-only steps:

1. Install the Ruby programming language:

        sudo apt-get install ruby-full

2. Install Bundler, a ruby package manager:

        gem install bundler

3. From a command prompt, navigate to your github pages repo directory

4. Bundler expects you to have a file called `Gemfile` in your repository root.
   And, for a GitHub Pages jekyll site, your Gemfile needs to reference the GitHub
   [`pages-gem`](https://github.com/github/pages-gem). So, create a Gemfile
   and set its content:

        cat <<EOF > Gemfile
        source "https://rubygems.org"
        gem "github-pages", group: :jekyll_plugins
        EOF

   Then, run `bundle install`. This will generate a new file called `Gemfile.lock`.

5. You will want to have a file in your repository called `.gitignore` which will prevent
   you from accidentally committing local-build-related to your github repository.
   Create a minimal jekyll-themed `.gitignore` file as follows:

        cat <<EOF > .gitignore
        _site
        .sass-cache
        .jekyll-cache
        .jekyll-metadata
        vendor
        EOF

6. Add and commit your new `Gemfile`, `Gemfile.lock`, `.gitignore` files to your repository.

Now, you can launch a jekyll server which will serve your github pages site
and dynamically rebuild it if any file changes are detected.

      bundle exec jekyll serve

Except, if you change your `_config.yml`, you will need to kill your jekyll server and
restart it.



#### Assuming you're on Windows, do everything from WSL Ubuntu Bash prompt

If you're on Windows, then _do everything from your WSL Ubuntu instance_. Recall
that to access your instance, open a command prompt and run the command `bash`.
That done, everything you do from that bash prompt will be done `on Ubuntu`.
Then, you can follow whatever website instructions for doing something _on Ubuntu_,
without exception.

For example, the guide will tell you to install Ruby. Do this from wsl-ubuntu-bash following
the `apt (Debian or Ubuntu)` instructions on <https://www.ruby-lang.org/en/documentation/installation/>:

    $ sudo apt-get install ruby-full
