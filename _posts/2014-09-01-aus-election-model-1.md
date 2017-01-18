---
layout: post
title: Difficulties in forecasting elections in Australia I - Introduction
image:
  feature: sample-image-16.jpg
  credit: Sydney, New South Wales, 2014

---

In recent years, statistical approaches to electoral forecasting have become particularly popular. The most famous example in the past decade has been the rise of Nate Silver and his FiveThirtyEight model, which has been immensely successful in predicting outcomes of American elections, as well as bringing the notion of precise and robust electoral forecasting to the forefront of public discourse.

In such an environment it is natural to ask why there has been no equally sophisticated and successful attempt to replicate Silver's approach outside of the United States. The answer is that, by and large, the United States as a country, and its political system, are particularly well-suited for the type of analysis provided by Silver. 

On the other hand, the Australian electoral system, and its child systems in various Australian states, together with Australia's geographic and demographic make up, make it particularly poorly suited for the robust regression analysis of opinion polls propounded by Silver. In this series of posts I want to discuss the main reasons for these difficulties, and present some comments and suggestions towards a solution. The hope is that I can recast the discussion towards formulating a different model, specifically designed for the Australian system, that is able to provide a similarly accurate and robust analysis of Australian elections (this has always been an ongoing long-term project of mine!). Such a model would ostensibly be adaptable to elections in the UK, with some changes (the UK also presents some other unique challenges). 

In order to begin discussing any kind of analysis of polls for Australian elections, it is necessary to concede that sample sizes and poll frequencies here are nowhere near those achieved in the United States. This will have an unavoidable and significant effect on accuracy of results and sizes of confidence intervals that **cannot be compensated for**, without actually physically conducting more polls. 

There is an understandable desire for electoral forecasting to be accurate. Nate Silver has done his own craft a disservice in a sense: his two phenomenally accurate predictions of US presidential elections were still to some extent the result of luck: there is always inaccuracy when attempting to predict a complicated process with limited data, and this is something that needs to be accepted will be a greater trade-off in the Australian case (I'm still dreading the time that one of Silver's forecasts is wrong and people write off the statistical method forever, even though he's bound to be wrong 1/20th of the time). Nevertheless, I am confident that a fair level of accuracy can be obtained with a careful approach that uses all possible data: 

- an average of opinion polls, weighted for pollster past accuracy
- demographic data at a seat level
- past election data
- redistrubution data
- preference data
- an economic index

to obtain probabilistic forecasts for each seat (this is my ultimate goal). 
