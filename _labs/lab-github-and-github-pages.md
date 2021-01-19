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

**Deliverable: submit a link to your github profile: e.g., https://github.com/deargle**

Now, complete the [GitHub Hello World project](https://guides.github.com/activities/hello-world/)

**Deliverable: submit a link to your github hello-world repository**

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
* Hook Atom up to GitHub ([the package comes bundled with Atom, because All Hail Octocat](https://github.atom.io/)).

Next, customize your `index.md` using the Markdown language, and then push your
local changes to your github-hosted GitHub Pages repository.

* Learn about Markdown. Specifically, read about [GitHub-flavored Markdown](https://guides.github.com/features/mastering-markdown/).

* Using Atom instead of the GitHub website proper, follow the instructions in
  [@evanca's instructions on Medium.](https://blog.usejournal.com/set-up-your-portfolio-website-in-less-than-10-minutes-with-github-pages-d0efa8ff56fd) **steps 6 through 9**. In these steps, you will do the following:

  - upload a profile picture
  - customize your `_config.yml`
  - customize your `index.md`

* Using Atom, commit and push your changes. Check that your website has updated! It might take a minute. You should
  get an email from github if you broke anything with your `_config.yml` edits.

**Deliverable: A link to your github pages website (e.g., https://deargle.github.io), and also to its backing repository (e.g., https://github.com/deargle/deargle.github.io)**

**Deliverable: Screenshot committing and pushing your changes, demonstrating local git workflow.**

Warning! Web dev is a sinkhole activity! You can spend _days_ on this, at the
expense of your other work. Just stick to getting the basics up, then _get out_.
