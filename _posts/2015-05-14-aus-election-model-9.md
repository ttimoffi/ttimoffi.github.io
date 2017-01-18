---
layout: post
title: Forecasting Australian elections III - Emma Chisit v1.0
image:
  feature: sample-image-31.jpg
  credit: Kosziusko National Park, New South Wales, 2012

---

I've spent the last few months of my spare time writing code for a [model](http://www.clintonboys.com/emma-chisit/) to forecast Australian elections. I'm calling the model Emma Chisit. A very basic first version of the model is finished, and I wanted to write up a few things about how it works, mainly so I can explain the current shortcomings, and the improvements I want to make down the line. The good thing is that I've written the bones of many of the core parts of the model, and have something that mostly works and is easy to improve. 

**The data used by the model**

The model uses the following data:

- poll data obtained from two main sources: The Phantom Trend's github repo which contains a nearly complete database of federal polling data from this century, and my own (incomplete but continuing) efforts at scraping the web for state polling data. At the moment I'm concentrating on federal elections, so my model is only using the federal polling data. 
- election results data from the AEC results page. The model uses the primary votes by polling place data for each polling place in the country
- preference data from Antony Green. I'm using Antony Green's calculations of preference flows in the 2013 election. 

**Poll aggregator to swings**

The current heart of the model is a poll aggregator. I'm using the same code that I wrote for my [Israeli poll aggregator](http://www.clintonboys.com/israel-poll-aggregator-1/), which accounts for pollster reliability (measured across my entire poll database, including state polls) and recency. The model computes a poll aggregate (which has a much greater historical tendency for accuracy than the individual pollsters) and then the implied swing (percentage change) from the previous election's results. 

**Four-party model and runoff simulator**

The model then takes the swing estimate and applies it to each individual seat. At this stage, the model assumes the system consists of four parties, the Coalition, the Labor Party, the Greens and a generic "Others" label that encompasses everyone else. This was a decision I made early on because these are the only four options reliably included in all polls. It's also the biggest shortcoming of the model in predicting recent elections, mainly because of the rise of PUP. 

**Current output**

I ran this basic version of the model using the polling data from the thirty days before the 2013 federal election, and the results from the 2010 federal election. The output looks like this:

    Greenway 49.48 | 52.98
    Hughes 43.72 | 39.33
    Hume 39.66 | 38.53
    Hunter 61.57 | 53.67
    Kingsford Smith 52.11 | 52.74
    Lindsay 49.02 | 47.01

with seat names, the predicted result in the seat, and the actual result in the seat at the election. Of the six seats above, the model gets five right and one (Greenway) wrong. In total, the model gets 134 out of 150 seats right and has an average error per seat of about 2.5%. 

**Shortcomings**

The model fails completely at the moment in seats where the top two contestants are not the ALP and the Coalition. This accounts for five seats. It also fails in seats with strong independent candidates, or seats with a high PUP vote. The average error, 2.5%, is too high, since usually more than 20% of seats are decided by a TPP margin smaller than this. 

**Next steps**

In order of priority, the next steps for the model are as follows:

- clean up the code so the modules work faster and together better (at the moment it has all been built ad hoc on top of itself so it's a little messy)
- add the ability for two-party preferred contests between parties other than the ALP and the Coalition
- add the option for a strong fourth party (like PUP in the 2013 election, and previous parties like Family First, One Nation and the Democrats)
- use historical data (and polling data) to better estimate [preference flows](http://www.clintonboys.com/aus-election-model-3/) (in particular, to obtain preference flows for non TPP contests)
- incorporate [clustering](http://www.clintonboys.com/aus-election-model-8/) by demographic data into the model to more accurately individualise the swing estimate to seat clusters
- incorporate state polls into the federal model
- incorporate marginal seat polls into the model
- account for personal votes of popular and long-incumbent members
- get the model working for individual states
- instead of providing a numerical estimate for each seat, provide a probabilistic estimate and a confidence interval
- account for [redistributions](http://www.clintonboys.com/aus-election-model-4/) by reassigning polling places
- add a trend feature to extrapolate results to the nearest election rather than just providing a nowcast
- incorporate an [economic index](http://www.clintonboys.com/aus-election-model-6/)
