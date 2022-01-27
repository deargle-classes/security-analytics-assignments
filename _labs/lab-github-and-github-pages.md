---
title: Lab -- Github, Markdown, and You
learning_objectives:

  - Create a simple resume using GitHub Pages
  - Write documents using Markdown
  - Demonstrate introductory mastery Git, Github, and Github Pages workflow
    using GitHub Desktop and Atom
  - Use Docker and docker-compose to run jekyll locally; show general ability to
    do containerized app development

order: 1
description: >

  Old people say "if you want a job, you must make a good LinkedIn profile." I
  say pfft. Make a good github profile instead (or other hosted code repo profile). DIY ftw.
---


This lab is split into three parts.
1. The first two parts do not require any command-line use -- they can be done
   exclusively from a browser.
1. The third part requires using the command line to run a docker container.
   * I'll post an introduction-to-the-command-line page, coming soon.

At the end is a rather lengthy "supplemental" section, with information on
things like buying and using custom domain names for your website, and blogging
with jekyll. These are pet topics for me, and they're literally _super cool_.
Try them if you dare!

# Part 1: Introduction to Github and Markdown

## Create a nice github account

Create a github account, and make it presentable.

Do the following:
1.  Choose a username that you won't mind appearing on a resume.
    * Github username are [changeable](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-user-account/managing-user-account-settings/changing-your-github-username).
1.  Set a nice profile picture for yourself, and set your name and
    organizational affiliation.
    * This helps others know they've found the right account, if they're
      searching for you.

<span class='badge badge-primary'>Lab requirement:</span> Make a github profile following the above guidelines.

## Do a "Hello World" github project

Now, complete the [GitHub "Hello World" project](https://guides.github.com/activities/hello-world/).
This will teach you how to use Github.


## Learn about writing in Markdown

[Plain text wherever possible](http://www.linfo.org/plain_text.html) is an
important part of the Unix Philosophy. [Markdown](https://daringfireball.net/projects/markdown/) follows that Philosophy. It is an "easy-to-read, easy-to-write plain text format" that can be converted into HTML.

Skim [both of these
guides](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github)
for an introduction to writing in Markdown on GitHub. You will need some of this
later in the lab.



# Part 2: Create a portfolio website

Github has a service called [Github Pages](https://pages.github.com/) for
creating websites. Skim that page.  They try to make the service easy-cheese to
use.

The default Github Pages "themes" are tailed for either blogs or projects. But
you want a portfolio!

For your portfolio website, I recommend that you use [@evanca's
'quick-portfolio' theme](https://github.com/evanca/quick-portfolio). To do so,
follow [@evanca's instructions on
Medium][medium-post].

[medium-post]: https://blog.usejournal.com/set-up-your-portfolio-website-in-less-than-10-minutes-with-github-pages-d0efa8ff56fd

<div class='alert alert-info'>
<p>
<strong>Side note:</strong> Evanca's theme is a customization of the <a href='https://github.com/pages-themes/minimal'>jekyll-theme-minimal</a> theme.</p>

<p class='mb-0'>Visit that link and skim the README for customization info. @evanca's theme
follows these instructions and makes a few small tweaks to make the site more
portfolio-related. If you wanted to
customize @evanca's theme further, you could follow the same instructions.</p>
</div>

<span class='badge badge-primary'>Lab requirement:</span> For this lab, you only need to get a basic website up with a
picture, your name, and a two-liner something-about-you quip.

But over the course of the semester, I'll have you link to at least three portfolio
pieces to your website. Due dates for those will be separate.


## How did I make a website without writing HTML?

Time for acronyms! Read this section, it helps to understand stuff later in this lab.

Websites can either be **dynamic**, or **static**.
* _Dynamic websites_ have a server that **does stuff** _before_ content is sent
  to browsers. The stuff it does _depends on_ content the browser sends it --
  like make the browser sends a login token, and the server pulls account info
  out of a database and returns it for that login. In that way, dynamic websites
  care about _state_.
* _Static websites_ don't care who is making the request. Everyone gets the same
  content. In that way, static websites are _stateless_ (from the server's
  perspective). (Note that the content can do dynamic stuff _after it arrives_,
  via javascript, but since the server didn't do anything dynamic it's still a
  static site.)

Cool vs uncool:
* Bloggers think it's cool to have **static** blogs. Static websites usually
  load _faster_ than dynamic websites, because there's no dynamic stuff
  involved.
* Static websites aren't susceptible to being _hacked_ in the same way that
  dynamic websites are. Dynamic websites like Wordpress or Drupal sites get
  pwned all the time because of things like add-ons with vulnerabilities. Some
  add-on vulns mean that attackers can execute arbitrary code _on the server_,
  which is a "bad thing."
* It's cool to have a cool-looking blog. That's one thing that makes dynamic
  websites cool -- html page content can be dynamically pieced together,
  permitting cool combinations of stylization and whatnot, with page _content_
  being pulled separately from stylization, from a database or something.
* It's _uncool_ to have to cram a bunch of styling information into _each and
  every static blog page._ That's one downside of static sites -- each request
  returns a _single file_. That means that every file has to include all
  stylization information, repeated ad nauseam for each file. Maintenance is a
  nightmare. It's _cool_ to make something else do all the stylization stuff.

Static Website Generators are the cool answer to all of the above. For writing,
content is kept separate from stylization. (Content can be written in Markdown.)
Point the generator at the project, and it will create one html file for each
possible webpage, merging together stylization and rendered content. Sounds
great! :sunglasses:

Under the hood, Github Pages runs a static website generator called
[Jekyll](https://jekyllrb.com/). Jekyll is, therefore, cool, by definition.

When you commit code to a github-pages-enabled repo, github runs jekyll against
it to generate the _static content_ that gets served when you visit the pages
url. This happens by default for any pages-enabled github repo, unless you
explicitly disable jekyll for your repo.


## Sign the class website yearbook

Once you have completed the "Hello World" project, skimmed the markdown guides, and launched your website, you should be able to sign the class yearbook.

Submit a pull request to edit the class repository.

Specifically, "sign" the class repo `index.md` page (find the yearbook URL on canvas).
Follow the example already there for "Dave Eargle" -- include a link to your
github profile (e.g., <https://github.com/deargle>), and to your built website (e.g., <https://deargle.github.io>). Once you have opened the pull request, submit a link to your pull requests.

<span class='badge badge-primary'>Requirements:</span> Your github profile and your website should meet the minimum
"lab requirement" criteria specified in the respective sections above.

{% include lab_question.html question='Submit a link to your pull request that signs the class yearbook.' %}



# Part 3: Developing Github Pages sites locally

It's a pain in the butt to have to wait for github to rebuild your website to
see change you're trying to develop. And what if it looks bad! Anyone on the
internet could see it!

Cool kids render their websites _locally_ instead, before pushing changes
_live_. Let's do that, using Docker.

<div class='alert alert-danger'><strong>Heads up!</strong> This section requires command-line usage.
If that makes you uncomfortable, then do the <a href='{{ site.baseurl }}{% link intro-to-command-line.md %}'>intro to command-line tutorial</a> first.</div>

## Install a nice text editor

You need a nice text editor. I like [Atom](https://atom.io/). For this lab,
download and install Atom. After, if you hate it, you can abandon it.

## Clone your repo

Next, you need a git client, and why not one that is also github-aware.

1.  Install [GitHub Desktop](https://desktop.github.com/).
1.  Then, use it to "clone" your github pages repo to your local computer.
    * For help, read [this guide](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/adding-and-cloning-repositories/cloning-and-forking-repositories-from-github-desktop).
1.  Then, use Github Desktop to open your repo in Atom. You'll edit it there later.


## Read about the jekyll/jekyll docker image

To build your site locally, you need to run [Jekyll](https://jekyllrb.com)
locally. Jekyll is a [ruby](https://www.ruby-lang.org/en/) app.

Skim [the jekyll install docs](https://jekyllrb.com/docs/installation/)
including the guide for your OS. Read until you realize that it looks painful to
install jekyll. And that's right, it _is_ painful!

This is _exactly_ the kind of pain that Docker exists to fix.

We will use the [`jekyll/jekyll`](https://hub.docker.com/r/jekyll/jekyll/)
docker image. This image already has jekyll installed inside it. We just need to
make the repo's files available to the running docker container, so that jekyll
can build the site.

Skim the image's
[README](https://github.com/envygeeks/jekyll-docker/blob/master/README.md), but
don't do anything yet.

<div class='alert alert-info'><strong>Is this repo defunct?</strong> If you
browse the repo's "Issues" and "Pull Requests," you would realize that this repo has many unaddressed issues. No matter, it works
anyhow.</div>



## Add a `Gemfile` to your project

Jekyll is a `ruby` program. `ruby` library packages are called "gems". A project can include a `Gemfile` to specify
the gems that it needs.

Use Atom to add a file called `Gemfile` to the root of your repo with the following contents:

```ruby
# Gemfile
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```

This says that our project needs the "github-pages" gem. That gem in turn
includes many other gems.


## Run jekyll docker container

Install [Docker-Desktop](https://www.docker.com/products/docker-desktop) if you
don't already have it.

Now, open a terminal in the directory of your cloned repo.
* You can see where your repo is located on your filesystem by using the "View
  the files of your repository" button in Github Desktop.
* You could use the Github Desktop `Repository > Open in Command Prompt/Terminal` menu item:

  {% include lab-image.html image='github-desktop-open-in-command-prompt.png' %}


Now, look at the following `docker` command, but don't run it yet. First, look at it in
multi-line form (this multiline form is not valid in Windows shell prompts):

```bash
docker run \
  --rm \
  -it \
  --volume="${PWD}:/srv/jekyll" \
  --publish 4000:4000 \
  jekyll/jekyll:3.8 \
  jekyll serve
```

Here is the same command, all on one line:

```bash
docker run --rm -it --volume="${PWD}:/srv/jekyll" --publish 4000:4000 jekyll/jekyll:3.8 jekyll serve
```


First, understand the command:
* The `run` command starts a docker container from image
  [`jekyll/jekyll`](https://hub.docker.com/r/jekyll/jekyll/) -- specifically,
  the version tagged as [`3.8`](https://hub.docker.com/r/jekyll/jekyll/tags).
* This will remove (`--rm`) the container when we stop it. This is desirable,
  since otherwise, each time running the command would create a new container.
* `-it` gives us an interactive shell which will permit stopping the container
  via `Ctrl-C`.
* The `--volume` (equivalent to `-v`) flag mounts the current directory (`${PWD}`) inside the
  container. This makes your website files available to the container.
* The `--publish` (equivalent to `-p`) flag maps container port `4000` to your computer's port
  `4000`. This means you will be able to access <http://localhost:4000> from a
  browser on the host to view whatever is being served on that port within the
  container.
* The last arguments are `jekyll serve`. This is the command that the container
  will ultimately run. It will launch a webserver that will (1) build the
  website, (2) serve the built website, and (3) watch for new changes so that it
  can rebuild. It's slick.

Now, run the earlier `docker` command. <i class="fas fa-rocket"></i>

From the output, you notice that the first thing the container does is install a
bunch of _gems_. Gems are ruby _packages_. While the image _already_ includes a
bunch of installed gems, the container looks inside our `Gemfile` and sees that
it needs to install the `github-pages` ones.

Yawn, this takes a while.

...but once that's done, you should see a message that your site is being served on
<http://localhost:4000>. Visit that url. Hopefully it's working! If not,
:no_entry_sign: stop and panic. Otherwise, continue.

Jekyll is watching for filesystem changes. Use Atom to make a change to a `.md`
file in your repo. Confirm that log output shows that the change was detected.
Refresh the browser to confirm that the update has been applied.

<div class='alert alert-warning'><strong>Heads up!</strong> Jekyll will not
auto-regenerate if you modify <code>_config.yml</code>.  You have to restart jekyll
to pick up changes to that file.</div>

Close the running container by running `Ctrl-C`.

Now, run the earlier docker command again (`up`-arrow to cycle through your shell's history!).

Oh no, it has to install the gems again! Why doesn't it remember? Because we
removed (`--rm`) the image. It starts fresh each time. That's a _feature_ of Docker.
But surely we can cache somehow?

## Caching gems using a filesystem mount

The [`jekyll/jekyll`
docs](https://github.com/envygeeks/jekyll-docker/blob/master/README.md) say that
a volume can be mounted to the container internal directory `/usr/local/bundle`.
Gems will be installed to that directory, and the container will look there
before installing them again. That's what we want!

But there's a gotcha for Windows users. The readme recommends the following flag:

```bash
--volume="${PWD}/vendor/bundle:/usr/local/bundle"
```

This would save installed gems to local project directory `vendor/bundle`. This
would in turn be included in the `${PWD}:/srv/jekyll` volume mount. But there can be a _lot_
of files in that directory. And on Windows, mounted directories get _really really slow_
-- like, jumping to a 5-minute-mount-time slow. Not good!

## Caching gems using a docker volume instead

A solution instead is to install the bundled files to a _docker volume_, which
are _not_ be mounted to the windows filesystem. (They stay within the docker engine.)

[Here's](https://docs.docker.com/storage/volumes/#start-a-container-with-a-volume)
the docker volume docs. It says that if we specify a volume that doesn't exist
yet, Docker will create it for us :+1:.

So, do that, naming the volume something like `my-bundle`:

```bash
docker run --rm -it --volume="my-bundle:/usr/local/bundle" --volume="${PWD}:/srv/jekyll" --publish 4000:4000 jekyll/jekyll:3.8 jekyll serve
```

Run the command and wait for the gems to install (stored in the volume). Then
kill the container, then run it again. It should skip the gem install step this
time, and be much faster. Kill it again and run it again, just
to feel the power. :tada:

**Best practice:** Add your docker command to a `README.md` file in your repo, so
you can remember it later.

## Use docker-compose instead

But that's a cumbersome command. Let's use
[`docker-compose`](https://docs.docker.com/compose/) instead of typing that
monster out. It already comes installed with Docker-Desktop.

Create a YAML file in the root of your repo called `docker-compose.yml` with the following contents:

```yaml
version: '3'

services:
  jekyll:
    image: jekyll/jekyll:3.8
    ports:
      - "4000:4000"
    volumes:
      - .:/srv/jekyll
      - my-bundle:/usr/local/bundle
    restart: unless-stopped
    command: jekyll serve

volumes:
  my-bundle:
    external: true
```

Then, from the shell, call `docker-compose up`, and wait a bit. It should work!

Close the running container with `Ctrl-C`.

Take a break :tropical_drink:

{% include lab_question.html question='Submit a screenshot of successfully running `docker-compose up` to launch your website locally.' %}

## Create a `.gitignore` file

Running the container creates a few folders and files in your local directory
that you don't want to commit to your repos, such as the following:

* A folder called `_site` which contains the built website.
* The `vendor/bundle` directory from earlier, if you ran that command
* A `Gemfile.lock` file which specifies and pins _all_ installed gems to
  specific versions

To avoid committing these to your repos, create a file called `.gitignore` with the following contents:

```gitignore
_site
vendor/
Gemfile.lock
.sass-cache
.jekyll-cache
.jekyll-metadata
```

## Commit changes and push

Once you are satisfied with your website modifications, you need to `commit` them
to your local repo, and then `push` (sync) those commits to your github copy of your
repo.

Review [this Github Desktop](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/making-changes-in-a-branch/committing-and-reviewing-changes-to-your-project)
guide for how to use add, commit, and push changes.

Following that guide, add and commit your modifications.
* Don't forget to commit your `.gitignore` and `Gemfile` files!
* Write good commit messages!

Once you've pushed your commits to github, github will (eventually) rebuild your
site. If it failed to build, you'll get an email with an error message.

{% include lab_question.html question='Submit a screenshot of successfully using Github Desktop to push commits from your local repo to github.' %}


<div class='alert alert-danger'>
<strong>:warning:Warning!:no_bicycles:</strong> Web dev is a sinkhole activity! You can spend <em>days</em> on this, at the
expense of your other work. Just stick to getting the basics up, then <em>get out</em>.
</div>

# Supplemental

Nothing below this point is required. But that doesn't mean it's not good stuff.

## Tips for following evanca's Medium post

In Evanca's Medium post, you upload a profile picture.

Setting image urls can be tricky.

Github has a special url format that will let you fetch a _raw file_ as opposed
to a _view of the file wrapped in the GitHub UI_.  To get the raw file, append
`?raw=true` to the url.

Try it for yourself using a browser:
1.  Visit the forked repository's logo file _without_ using
    `?raw=true` here:
    <https://github.com/evanca/quick-portfolio/blob/master/images/demo.gif>. Note
    that it is wrapped in the GitHub UI.
1.  Then, visit it _with_ `?raw=true` appended:
    <https://github.com/evanca/quick-portfolio/blob/master/images/demo.gif?raw=true>.
    You get the raw image file. This is what you need, so that your logo can be
    displayed in your website.

Note that the forked repository follows the above pattern:

```yaml
# in _config.yml
logo: "/images/logo.png?raw=true"
```

Even though the log file stored in their repository's `/images/` folder is named just `logo.png`.


## Using a custom domain for your `<username>.github.io` site

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
3. ???
4. Profit.


## Connect Atom to Github

Hook Atom up to GitHub ([the package comes bundled with Atom, because All Hail Octocat](https://github.atom.io/)).

Click on the Github tab towards the bottom-right of your Atom window. Complete its
login form.

![atom-github-tab]({{ 'assets/images/atom-github-tab.png' | relative_url }})

Now, you can do the git "add", "commit", and "push" workflow from within Atom,
without having to use Github Desktop.


## Use Jekyll for Blogging

You're in luck! Jekyll's core competency is blogging! It's not hard to tack
blogging onto your `quick-portfolio` theme. Follow my silly guide below:

### Create a blog index page

Create a new file in the root of your repository to serve as an index to your
blog posts. Let's call it `blog.md`. You might make it look like this:

{% raw %}
~~~html
---
---

<h1>Blog</h1>

<ul>
  {% for post in site.posts %}
  <li>
    <a href='{{ post.url | relative_url }}'>{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>
~~~
{% endraw %}

The two sets of three hyphens at the top are called Front Matter, and they're
important. Read a [jekyll
guide](https://jekyllrb.com/docs/step-by-step/01-setup/) to understand why.

This index page will loop through your site's collection of blog posts, and render a link
to each one. The page will be available at <http://localhost:4000/blog>. Go ahead and visit it.

Except for the nav bar on the left, it's empty! That's because you don't _have_ any blog posts yet!

### Write a blog post.

Create a new folder in your repository exactly called `_posts`. Folders in the
root of your directory contain what
Jekyll calls a "collection." By default, Jekyll has special rules for handling
a collection called "posts".

Blog posts go in the "posts" collection -- one file per blog post. Create a new
file in this folder with a name using the following format:

~~~
MMMM-YY-DD-title-of-the-blog-post.md
~~~

Where the `.md` extension stands for "markdown." Its content also must start with two sets of three hyphens. After those hyphens,
write the blog post content, using markdown (or html). For example:

{% raw %}
```markdown
---
---

This is a blog post called `2020-01-25-My-dog-ate-my-homework.md`.
It is a true story about how my dog at my _homework_, **again!**.

And here is a second paragraph going on about my doggo woes.
```
{% endraw %}

Save the file. Then, navigate again to your blog index at <http://localhost:4000/blog>.

You should see your new blog post listed there! Our blog index read the title,
which it extrapolated from the filename.

Click the blog post to view it. It looks kind-of okay! But there's a phantom "By" line, and an
empty "tags" list. Let's fix both of those.

Jekyll uses "layouts" to determine common styling for multiple pages, so that
you don't have to repeat yourself across files. It makes updating styles easier.
The style for your theme's posts is found in the jekyll _theme_ that `quick-portfolio`
uses. (While the quick-portfolio theme overrode the "_layouts/default.html"
theme, it did _not_ override the theme used for posts. So we must look to the default.)

By examining `_config.yml`, we see that the theme being used is
[jekyll-minimal-theme](https://github.com/pages-themes/minimal/), and [its
layout for posts is found
here](https://github.com/pages-themes/minimal/blob/master/_layouts/post.html).
Follow the link to see the posts layout.


### Fixing the By-line

Examine the posts layout. Notice the sets of double curly braces. These are Jekyll
(Liquid) templating
placeholders. For the "By" line, we see that the theme is referencing the
`page.author` and `site.author` variables:

{% raw %}
```yaml
{{ page.author | default: site.author }}
```
{% endraw %}

`page.author` means that if our blog post had an author set, it would have been
set in the post Front Matter, between the sets of hyphens. Front Matter is
written in a config language called YAML. Like this:

```yaml
---
author: John Doe
---
```

But it would be lame to have to repeat that for each blog post. We look back to
the theme and notice that it references `default: site.author`. In jekyll,
`site.` variables are read from `_config.yml`. So, add a key-value entry in your
`_config.yml` file for `author`:

~~~yaml
author: put your name here
~~~

Restart your local jekyll server to read in changes to `_config.yml`. Reload the
blog post. Your name should be there!

### Fixing Post Tags

Examine the default posts layout again. It includes the following snippet:

{% raw %}
```html
{% if page.tags %}
<small>tags: <em>{{ page.tags | join: "</em> - <em>" }}</em></small>
{% endif %}
```
{% endraw %}

We see that it's looking for `page.tags`. [Read the jekyll docs on specifying tags](https://jekyllrb.com/docs/posts/#tags-and-categories). We see that
in our posts' front matter, we can
specify either a `tags` key or a `tag` key.

For this example, let's set the
latter, leading to a blog post that looks like this:

{% raw %}
```markdown
---
tag: lies
---

This is a blog post called `2020-01-25-My-dog-ate-my-homework.md`.
It is a true story about how my dog at my _homework_, **again!**.

And here is a second paragraph going on about my doggo woes.
```
{% endraw %}

Jekyll should have automatically detected your changes and rebuilt your site.
Refresh your blog post webpage. You should see that it lists its tag now!

Multiple tags are left as an exercise to the reader.

### Adding blog post dates to the blog index

Spicing up our blog index a little bit, let's list the blog date in the url.

A post has its date available as a variable, acessible via `.date`. And, the
templating language for Jekyll is a fork of the language called Liquid.
Liquid includes [_filters_](https://jekyllrb.com/docs/liquid/filters/), including
`date_to_string`. We can use it by piping in `post.date` and setting some
arguments, like this:

{% raw %}
```liquid
{{ post.title }} | {{ post.date | date_to_string: "ordinal", "US" }}
```
{% endraw %}

Making our blog index look like this:

{% raw %}
```html
---
---

<h1>Blog</h1>

<ul>
 {% for post in site.posts %}
 <li>
   <a href='{{ post.url | relative_url }}'>{{ post.title }}</a>
 </li>
 {% endfor %}
</ul>
```
{% endraw %}

Not bad!

### Add a link to the blog index

But how will anyone find our blog index? Let's modify `_layouts/default.html`
to include a link.

I added this line at about line 33, after the closing `endif` to the check
for whether the site is a github user page, like this:

{% raw %}
```html
... snipped ...

{% if site.github.is_user_page %}
<p class="view"><a href="{{ site.github.owner_url }}">View My GitHub Profile</a></p>
{% endif %}

<p class='view'><a href="{{ '/blog' | relative_url }}">View my Blog</a></p>

... snipped ...
```
{% endraw %}

Refresh your homepage, and you should see a link to your blog post on every page.

### Live example

I forked the `quick-porfolio` theme and made the above changes to it to enable
blogging. [View the fork here](https://github.com/deargle/quick-portfolio),
and [examine the blog-setting-up commit here](https://github.com/deargle/quick-portfolio/commit/45542845516a11254ba6df62b86cddad95f7b7af).

### Conclusion

The above is an insultingly shallow overview of blogging with jekyll. But it
should show you how you can make some small tweaks to your layout to quickly set
up with blogging. Hopefully it inspired you! Ask me for clarification and I'll
come back and update this doc. Glhf!
