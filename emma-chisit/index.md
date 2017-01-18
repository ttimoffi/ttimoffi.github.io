---
layout: page
title: Emma Chisit
image:
  feature: sample-image-32.jpg
  credit: Orange, New South Wales, 2013
---

Emma Chisit is a data-driven model for predicting and analysing Australian federal and state elections that I'm currently developing. 

**Why the name?**

Emma Chisit is Australian slang for "how much is it?". In an Australian accent, both expressions sound exactly the same. The phrase has its origins <a href="http://en.wikipedia.org/wiki/Monica_Dickens" target = "_blank">here</a>. I'm using this name because the model aims to give a probabilistic, clearly quantified measure of "how much" the current government is leading by in the polls, or "how many" seats they would win in an election.   

**What does it aim to do?**

The aim of the model is twofold:

- outside election campaigns, provide a sophisticated poll aggregator that takes into account pollster accuracy, and provides a probabilistic "nowcast" for **each seat** in the legislature
- within an election campaign, providing the above, together with a trend-based forecast

Ideally, I'd like to have the model be fully automated, including at the level of scraping the web to update itself to include new polls. I'd like to provide some analysis of its results when reported. It's worth noting that, as far as I am aware, this would be a completely different approach to any that [currently exist](http://www.clintonboys.com/aus-election-model-5/). 

**How is it different from existing models?**

Emma is based on the assumption that Australian elections are won at polling places. In particular, if we can obtain an estimate of the swing on a polling-place by polling-place basis, we can aggregate everything to obtain estimates of individual seat swings, and finally a full national picture of the election. This is done by clustering seats and polling places together according to demographic variables which then provide local adjustments to a national swing obtained by a robust poll aggregator. Everything is done probabilistically. 

**What's the current state of the model?**

As various pieces of the model are built, I'll update this section. Usually there will be an accompanying blog post for the section. 

29/3/15: As I only have a database of federal polling data, I have spent five days scraping the web for state poll data.

31/3/15: First version of [pollster weight calculator](http://www.clintonboys.com/aus-election-model-7/) is built. 

5/4/15: Working on an object-oriented skeleton of the model which will initially work with a few dummy polling places and seats. 

19/4/15: Read about some models that have similar ideas to mine; in particular the [strong transition model](http://www.electoralcalculus.co.uk/strongmodel.html) used by the may2015.co.uk site. 

20/4/15: Coded up a first draft of a runoff simulator. Incorporated some more poll data courtesy of William Bowe into the model. 

28/4/15: Incorporated the last of William Bowe's poll data. Translated old R code for clustering to Python. 

29/4/15: Started work on poll aggregation functions. Redrafted pollster weight calculator. 

30/4/15: Worked on various functions to load election data by polling place. Got some more data. 

14/5/15: Started work on first model for translating poll aggregator to individual seat swings. 

17/5/15: Fixing various bugs in previous components that I introduced. Fixed some data so that everything is consistent. 

20/5/15: All modules now work as intended. Started work on preference flow calculator. 

21/5/15: First pass of the model gets 133/150 seats in the 2013 Federal election correct. The main point of failure is not including PUP as a bone fide fourth party, so improving this and better calculating preference flows are the next steps. 

7/6/15: Rewrote lots of code for speed, removed old parts. 

27/6/15: Scraped marginal seat data from Ghost Who Votes. Planning to combine this with clustering. 

9/7/15: Started work on including marginal polls in the model.

2/9/15: Registered Emma Chisit's new domain. 

8/11/15: Worked on incorporating marginal seats into the model using the strong transition model. 

**Where can I see the source code?**

I'm keeping everything open source. It's all available on the [Github repo](https://github.com/clintonboys/emma-chisit) for this project. 

**When will it be finished?**

My ultimate goal is to have the model fully ready for the next federal election in the second half of 2016. There are plenty of steps to complete in the meantime. I'll keep the "what is the current state" section above updated as the model progresses. 