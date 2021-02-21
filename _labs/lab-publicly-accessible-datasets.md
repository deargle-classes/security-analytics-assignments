---
title: Lab -- Publicly Accessible Datasets
learning_objectives:
  - Demonstrate sharing datasets in publicly available ways
order: 4
---

Open data science means that others should be able to rereun your code and replicate your results.
The first hurdle to that is making your data pubilcy accessible.
I'm ignoring data access _rights_ and terms of use and just talking about streamlining the access ability.

Goals: should be one-click for the user, which means the following are **_out_**:
* the user mouting a shared google drive
* the user downloading data from kaggle et al and putting it in a certain working directory

Maybe-viable options include the following:
* Find it in an already-publicy-accessible place
  * e.g., OpenML datasets can be direct-downloaded via requesting a special url
* host the data on GitHub.
  * Caveat -- 25MB (Mb?) filesize limit.
* host the data on a personal cloud storage provider -- google drive, dropbox, etc.
  * Caveat -- some trickery is required to get a direct access link.
* host the data on a cloud computing platform, such as google cloud or aws s3.
  * Caveat -- requires the code-creator to maintain a cloud computing account. But arguably that's a _fringe positive_ skill to demonstrate for a data scientists.

Let's try all!

## Already-hosted datasets, such as OpenML or UCI ML

You can use url 'hacking' (h@x0r-ing) to extract direct-download links from places where datasets are already hosted, as long as the download link does not require authentication. (You _could_ still programmatically get links that require authentication, but that doesn't work for Open Data Science without sharing your username-password).

Let's try with an OpenML dataset such as the [spambase dataset](https://www.openml.org/d/44). Look at that dataset page's url:

<https://www.openml.org/d/44>

We intuit that the dataset id is `44`.

On the dataset's page, there's a cute little cloud download icon with "CSV" under it towards the top right of the page. Hover over it to see the link address. The one for this dataset is the following:

<https://www.openml.org/data/get_csv/44/dataset_44_spambase.arff>

If you click that url, the csv should download.

What is an `.arff` file, you ask? Well, it doesn't matter, as long as it's structured tabularly or csv-ily. If you can open the downloaded file in e.g. Excel, it will work for [`pd.read_csv`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html) and the like. So forget about it!.

Right-click it and select "Copy link adddress" (at least in Chrome, all browsers have something similar).

Paste this url into a new browser tab. If it downloads a csv to your browser, then tada! you have a direct-download link.

Imagine you were interested in generalizing the url pattern. Note the URL pattern. It's something like:

``https://www.openml.org/data/get_csv/<dataset_id>/<some_specific_filename>``

It's easy enough to generalize where to insert the dataset_id, but I'm nervous about the `<some_specific_filename>`. Let's take a guess though and see if the url works _without_ providing the specific filename -- maybe the site's api will default to providing some default csv for the given dataset. Edit the url to just be the following:

<https://www.openml.org/data/get_csv/44/>

And paste it into a browser. Yay, it works.

Know that some web servers may be picky about url routing -- for example, it might not work without a trailing slash. We don't know what web server openML is using, but we can black-box test it. Try without a trailing slash:

<https://www.openml.org/data/get_csv/44>

It still works, at least for this site.

What this means is that you could use the above url directly in your script via a call such as:

```python
import pandas as pd
pd.read_csv('https://www.openml.org/data/get_csv/44')
```

If your datasets are not already available publicly somewhere else, you can do one of the following.

## Uploading small datasets to github

You can host your own datasets that are < 25 MB on github. For demonstration purposes, I committed the above spambase dataset to github. View it at <https://github.com/deargle-classes/security-analytics-assignments/blob/main/datasets/dataset_44_spambase.csv>.

I _cannot_ use the above as a direct download link. It would download just what you see in your browser -- an html wrapper around the dataset. I need just the dataset! Github provides a convenient `?raw=true` url argument you can append to get a "raw" file, not-wrapped in html. Append that to the above url, and click it, and see if you get the csv:

<https://github.com/deargle-classes/security-analytics-assignments/blob/main/datasets/dataset_44_spambase.csv?raw=true>

Hurray, yes you do. Note the resolved url in your address bar after you click the above link:

<https://raw.githubusercontent.com/deargle-classes/security-analytics-assignments/main/datasets/dataset_44_spambase.csv>

We infer that `?raw=true` redirects us to a `raw.githubusercontent.com` path. We _could_ alternatively infer the pattern from the above link, and get the same result in our analytics script.

_Also_ note that the html-wrapped view of the file has a "raw" button on it, above the dataset to the right. Hover over and inspect where that button would take you:

<https://github.com/deargle-classes/security-analytics-assignments/raw/main/datasets/dataset_44_spambase.csv>

This url says `/raw` instead of `/blob`. Click the url and note that it redirects you to the above `raw.githubusercontent.com` link. You could infer a different workable `/raw` pattern from the above.

(Working as of 2/16/2021).

## Sharing larger datasets from Google Drive or Dropbox

You can manipulate sharing links from Google Drive or Dropbox to get direct download links that you can use in scripts.

### Google Drive

Using the browser <https://drive.google.com> view, I uploaded the spambase dataset to my personal google drive, in a folder I created called "datasets".

Right-click the file and select "Get link." In the popup window, change the access rights to be that "anyone with the link" can view. Copy the link to your clipboard. My link looks like this:

<https://drive.google.com/file/d/19xCOJyKJ-VSCGL-cM2pwY3fCVw-yzMsM/view?usp=sharing>

Our goal is to convert the above into a direct download link. This one is trickier than GitHub's. When I visit the above link in my browser, I see a promising "download" button on the top right. But I don't get a url when I hover over it. Ah, Google Drive using javascript magic!

When I click on the download button, I notice a new tab open, and then close. Like a ninja I copied the url out of the new tab before it closed, and I got this:

<https://drive.google.com/u/0/uc?id=19xCOJyKJ-VSCGL-cM2pwY3fCVw-yzMsM&export=download>

https://drive.google.com/uc?export=download&id=19xCOJyKJ-VSCGL-cM2pwY3fCVw-yzMsM


This url pattern seems harder to generalize from, but I notice that the `id` in the second url is also present in the first url. Taking a guess, I'm crossing my fingers that the pattern is as simple as the following for all files, but I would need to confirm!:

``https://drive.google.com/u/0/uc?id=<id_pulled_from_first_url>&export=download``

[This SO post](https://webapps.stackexchange.com/questions/33997/is-there-a-direct-download-links-for-files-on-google-drive) says that the url pattern _does_ generalize, with the slightly simpler form as follows:

```
https://docs.google.com/uc?export=download&id=YourIndividualID
```

### Dropbox

If you get a public "sharing link" for a file in dropbox and examine the URL, you'll notice that it looks like this:

`https://www.dropbox.com/s/611argvbp5dyebw/HealthInfoBreaches.csv?dl=0`

I took a guess and changed the `dl=0` at the end to `dl=1`, and tada it direct-downloaded! Like this:

<https://www.dropbox.com/s/611argvbp5dyebw/HealthInfoBreaches.csv?dl=1>

Easy enough.

## AWS S3

You can create an AWS S3 "bucket" from which you host public datasets. The s3 stands for "simple storage service".
The process to create a bucket is hardly "simple," but it _is_ one-time. Charges,
if any, should be extremely minimal (less than 10 cents a month?).

* Create an AWS account if you don't have one already
* Navigate to the S3 Service
* I recommend creating a new bucket if you don't have one already. Its name must be globally unique across all of S3.
  I created one called `deargle-public-datasets`.

  This bucket is intended for open data science replication, so your bucket needs to allow public access. So turn _off_ "Block public access." I left on other bucket defaults.


* However, the above on its own will not make your files publicly accessible. It just
  makes it so they _can_ be made publicly accessible.

  On the bucket "Permissions" tab, scroll down to the "Bucket Policy" area.
  This is the area where you write policies using JSON. Click "Edit,"
  and paste in the following policy, replacing the value for the "Resource"
  key with the ARN shown for your bucket immediately above the Edit pane.
  **Leave the trailing `/*` at the end of your resource name.**

  ~~~json
  {
      "Version": "2008-10-17",
      "Statement": [
          {
              "Sid": "AllowPublicRead",
              "Effect": "Allow",
              "Principal": {
                  "AWS": "*"
              },
              "Action": "s3:GetObject",
              "Resource": "arn:aws:s3:::deargle-public-datasets/*"
          }
      ]
  }
  ~~~

  You will get a lot of very scary warnings about how your bucket is now _public, public public!_. This is desired for our open-data-science use case. **Only upload datasets to this bucket that you want the world to have read-access to.**

Next:
* upload your dataset to the bucket
* Navigate to the page for your newly-uploaded file by clicking on the filename. On this page, you can find your
  file's "Object URL." To test that anyone can access your dataset using this URL, copy-paste this into a browser window in which you are _not_ logged in to AWS (for instance, into an incognito or private browsing window).

  If you set the policy correctly, your file should download. This means that you can use this url in your analytics script.


## Google Cloud

Similar to AWS S3, Google Cloud also uses 'buckets' from which you can host public datasets. Relative to AWS, the process to share files from a GCP bucket is extremely simple.

* Create a GCP account if you don't have one already
* Open your GCP web console (<https://console.cloud.google.com/>), and select a project.
    - Unlike AWS where s3 buckets are specific to accounts, buckets in GCP are specific to _projects_
* Proceed to Storage -> Browser in the navigation pane and create a new bucket.
    - I created one called `deargle-public-datasets`
    - For "Choose where to store your data," I left the default
    - For "Choose a default storage class for your data," I left the default
    - For "Choose how to control access to objects," I chose _Uniform_
* Read and follow the GCP documentation for [Making all objects in the bucket publicly readable](https://cloud.google.com/storage/docs/access-control/making-data-public#buckets). As of 2/20/2021,
  those instructions are the following:

  ![gcp-make-all-objects-in-bucket-publicly-accessible]({{ '/assets/images/gcp-make-all-objects-in-bucket-publicly-accessible.png' | relative_url }})
* Ignore all scary warnings about this bucket's public-ness, past present and future
* Click back to the "Objects" tab and upload your files to your new bucket
* When the upload is complete, click on the file to view object details. You should see a "Public URL"
  for your dataset. Navigate to this url using a private-browsing window to ensure that
  your dataset is publicly-available.
* Use this url with `pd.read_csv`.

## Deliverable

[Use this jupyter notebook template](https://colab.research.google.com/drive/1O-C9cPIDTqSXO62qkcEdKUQxCLAsbSv5?usp=sharing)

Share a jupyter notebook in which you demonstrate loading the [OpenML phish_url dataset](https://www.openml.org/d/42641), (~5MB) into a pandas dataframe in **each** of the following ways:

* OpenML link
* GitHub
* Google Drive or Dropbox or another personal cloud storage method
* AWS S3 or Google Cloud or another cloud computing storage service

Each method should be in its own cell. The output of each cell should be the dataset.

Use the template deliverable -- make a copy of it and fill it in.
