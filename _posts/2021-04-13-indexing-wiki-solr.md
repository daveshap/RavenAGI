---
layout: default
title: "Indexing Wikipedia with SOLR"
date: 2021-04-13
description: SOLR will be a key technology for Raven
---

# Indexing Wikipedia with SOLR

[https://github.com/daveshap/PlainTextWikipedia](https://github.com/daveshap/PlainTextWikipedia)

<iframe width="560" height="315" src="https://www.youtube.com/embed/tKJRt809JMc" title="Indexing Wikipedia with SOLR" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## What is SOLR?

[https://solr.apache.org/](https://solr.apache.org/)

> Solr is the popular, blazing-fast, open source enterprise search platform built on Apache Lucene™.

Basically, SOLR is a search engine. I run it in [Docker Desktop](https://www.docker.com/products/docker-desktop). 

## How will SOLR be used?

First, I'm using SOLR to index a local copy of Wikipedia. This will serve as Raven's first source of **primary knowledge**. This is necessary because neural networks might have some **embedded knowledge**, but they also have a lot of other junk embedded, like racism and sexism. Neural networks are still fundamentally **language models**. This means they are not necessarily the best thing to rely on when it comes to cold hard facts. Language models, when trained on internet text, should be considered dubious sources of knowledge and facts. Storing encyclopedic knowledge in SOLR just makes sense. It means that Raven will be able to conjure up factual information about the world very quickly. Eventually, I would like to have multiple sources of primary knowledge, as there are known problems with Wikipedia. But Wikipedia is a great starting place because it is free, open-source, and crowd-sourced. 

Second, I'm going to use SOLR to record Raven's nexus. This means that Raven will have a chronological record of all input, thoughts, and output. Effectively, this will be the same as long-term memory for humans. This means that Raven will be able to recall every observation, conversation and idea that it's ever had. A side benefit of the recall service is that the data contained within can be used to train new neural networks. This process can be automated, granting Raven the ability to spontaneously learn - a key feature of true intelligence! 

## Parsing Wikipedia to Plaintext

Wikipedia is stored in databases in a format called *Wikimedia Markdown*. This is a nightmare syntax, a fusion of Markdown, HTML, and XML. Here's an example of what it looks like:

```xml
  <page>
    <title>April</title>
    <ns>0</ns>
    <id>1</id>
    <revision>
      <id>7158091</id>
      <parentid>7158078</parentid>
      <timestamp>2020-10-24T05:14:25Z</timestamp>
      <contributor>
        <username>Rsk6400</username>
        <id>1000261</id>
      </contributor>
      <comment>Reverted to revision 7141372 by Peterdownunder: Unexplained change of numbers. ([[WP:TW|TW]])</comment>
      <model>wikitext</model>
      <format>text/x-wiki</format>
      <text bytes="21804" xml:space="preserve">{{monththisyear|4}}
'''April''' is the fourth [[month]] of the [[year]], and comes between [[March]] and [[May]]. It is one of four months to have 30 [[day]]s.

April always begins on the same day of week as [[July]], and additionally, [[January]] in leap years. April always ends on the same day of the week as [[December]].

April's [[flower]]s are the [[Sweet Pea]] and [[Asteraceae|Daisy]]. Its [[birthstone]] is the [[diamond]]. The meaning of the diamond is innocence.

== The Month ==
[[File:Colorful spring garden.jpg|thumb|180px|right|[[Spring]] flowers in April in the [[Northern Hemisphere]].]]
April comes between [[March]] and [[May]], making it the fourth month of the year. It also comes first in the year out of the four months that have 30 days, as [[June]], [[September]] and [[November]] are later in the year.
```

As you can see, it's got all sorts of junk in it that make it less-than-readable. I want to parse this out into plain text so that Raven can read it easily without having to render and parse it. Enter regex. I've got a slew of functions in the GitHub repo listed at the top. Here's an example of one of the functions:

```python
def remove_citations(text):
    citations = re.findall('''\{\{.*?\}\}''', text)
    for cite in citations:
        text = text.replace(cite, ' ')
    return text
```

I won't detail all the functions, but you can take a look at them here: [https://github.com/daveshap/PlainTextWikipedia/blob/main/dewiki_functions.py](https://github.com/daveshap/PlainTextWikipedia/blob/main/dewiki_functions.py)

## Indexing Wikipedia to SOLR

This turned out to be far easier than I thought it would be. Imagine that! I powerful, open-source tool is easy to use! Here's the function I used to index all of Simple English Wikipedia in about 45 minutes:

```python
def solr_index(payload, core='wiki'):
    headers = {'Content-Type':'application/json'}
    if isinstance(payload, dict):  # individual JSON doc
        solr_api = 'http://localhost:8983/solr/%s/update/json/docs?commitWithin=5000' % core
    elif isinstance(payload, list):  # list of JSON docs
        solr_api = 'http://localhost:8983/solr/%s/update?commitWithin=5000' % core
    count = 0
    while True:
        count += 1
        if count > 6:
            return None
        try:
            resp = requests.request(method='POST', url=solr_api, json=payload, headers=headers)
            return resp
        except Exception as oops:
            print(oops)
            sleep(1)
```

There are two things to note here. The first is the `commitWithin` bit of the URL string. This query string tells SOLR to go ahead and automatically store the data I am sending it. The second is the `while True` loop. I found that sending articles to SOLR as fast as possible caused some to time out. With Python, one rule of thumb is just try something and handle the error rather than check to see if it worked. In this loop, I try to POST the data a few times. If it fails, it will wait 1 second and then try again. If it fails 6 times, it will give up on the article. Most of the time, it simply timed out due to SOLR being busy, and succeeded with 1 or 2 retries. I saw one article take 3 retries. So far as I saw, there were no other reasons for failure. 

## Encyclopedic Services

The encyclopedic services for Raven will be absolutely critical for Raven to be useful and reliable. These services will contain all kinds of information, such as general purpose information about the world as well as domain-specific knowledge. For instance, I would eventually like to have a medical encyclopedia service for Raven. Does that mean Raven will replace all doctors? No, not for a long time. But Raven could absolutely augment hospitals and doctor's offices. 

Whatever you want Raven to be able to do, there will be an encyclopedic service for. Are you a history buff? Let's load up Raven with every history book ever written! Are you an astronomy nerd? We can handle that too!

Eventually, Raven will be able to handle text, audio, and video, so the encyclopedic services are going to get huge. For now we're limiting it to just text. 

Here's the thing: Humans can master pretty much any topic by reading about it. Why not Raven? With powerful enough neural networks to "read" and interpret the information, Raven should be able to achieve human-or-better levels of mastery. 

## Recall Services

Neural networks have no long-term memory on their own. Your brain has unique structures to handle long-term recall, so why shouldn't Raven? Recall services will give Raven explicit, episodic memory of interacting with you as well as every thought Raven has ever had. Furthermore, this data can be used to fine tune and train new neural networks. Over time, Raven will learn to interact with you individually. Raven will intrinsically learn how you communicate as well as about your life and your needs. More on these services later. 