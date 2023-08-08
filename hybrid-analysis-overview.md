---
title: Hybrid Analysis and its uses
---

To quote their FAQ, “This webpage is a free malware analysis service for the community. Using this service you can submit files for in-depth static and dynamic analysis.”  Which is true, malware files can be submitted to their site for analysis. However, possibly a more useful tool, they maintain a somewhat useful API that can be used to query their database to _possibly_ create your own case specific datasets.

# Their Docs, overview
Check out [their docs](https://www.hybrid-analysis.com/docs/api/v2). Hybrid
Analysis produces a product they call Falcon Sandbox. Which is a much more
verbose system that does most of the dynamic analysis behind the scenes. What is
very cool about their website-product integration is when items are submitted for
analysis by a user of their product - Falcon Sandbox by default generates a
high-level report of the item in question. To include interesting features of
the item, whether that be signs of attempted access escalation, known malicious
artifacts are present, or process spawning. These high level reports are
available for query through their API. It also includes a threat score out of
one-hundred and a final label in the list of malicious, suspicious, no specific
threat, no verdict, or whitelisted.

# Regular Quickscans vs Falcon Reports
When somebody without a subscription to Falcon Sandbox submits a file for review
it runs a quickscan on the file/url/item in question similarly to other online
antivirus quickscans.

[//]:#![Hybrid-Analysis-WanaQuickScan]({{ '/assets/images/WanaQuickScan.png' | relative_url }})

The report shows their threatscore, the files category
(malicious, suspicious, etc.) and other identifying information about the file.
Their free system will tell you whether or not something is a dangerous item.
However, if we scroll down - on some items the Falcon Sandbox report will also
be readable. A Falcon Sandbox report is generated whenever a subscriber to the
system uses it. These reports are far more detailed than the quickscans, however
since to create one it requires a subscription to their software certain items
may not have reports available.

[//]:#![Hybrid-Analysis-WanaSandboxReport]({{ '/assets/images/WanaSandboxReport.png' | relative_url }})

[//]:#![Hybrid-Analysis-WanaFirstReport]({{ '/assets/images/WanaFirstReport.png' | relative_url }})

# The API
Their docs point out the existence of both a python and ruby wrapper
that can interact with their API, however in my experience the most efficient way to interact
with their data is to submit GET and POST requests directly to their API site,
which uses cURL to retrieve the data.

[//]:#![Hybrid-Analysis-API-CURL]({{ '/assets/images/API-CURL.png' | relative_url }})

Here you can see that the specified query terms when using their web API creates
a cURL line that is sent to the request URL also visible. It's possible to write
python code using the requests library to query their database.

# The two reports needed
Two forms of data are required from Hybrid Analysis to form a dataset with
a target and features. Both the threatscore which is acquired with a POST request
to this url - 'https://www.hybrid-analysis.com/api/v2/search/terms'
and the dictionary of feature information which requires a GET request to
this url - 'https://www.hybrid-analysis.com/api/v2/report/{input_job_id_here}/report/misp-json'

Quickscans will _sometimes_ have job_id's, which can then be mapped to an accompanying
Falcon Sandbox report that has feature information for the file mentioned in the quickscan
or one that is similar enough to it. Therefore it is necessary to get the job_id's before
accessing reports.

# Gotcha moments
Their servers are temperamental and don't always play nice, queries will mysteriously fail sometimes
and will also fail under these circumstances:
- Too many requests (get or post) too quickly - even if the speed does not exceed your API limit
- Requests over too large a window (time) - this will exceed your API limit and the returns will be empty nonsense
- You can only return 200 reports a minute, so if your query structure ever exceeds this the returns will be filled with nothing.
  - I _believe_ that the API limit is not how many JSON files you return but how many reports are in the return.
    peep the "count" line in the api-curl image, it says 140, so if another pull within the same minute makes you
    exceed 200 there, you lose.
