---
layout: post
title: Difficulties in forecasting elections in Australia III - Preferential voting
image:
  feature: sample-image-17.jpg
  credit: Mt Ossa, Tasmania, 2014

---


A successful forecasting model for elections in Australia needs to take into account its unique preferential voting system. In federal elections, voters are required to number all candidates in order of preference: only numbering a first preference results in an informal vote (in various states, preferential voting is optional, and there is a significant movement, particularly among prominent psephologists, to move to optional preferential voting on a federal level).

The deciding metric in the election of an Australian Member of Parliament is his/her two-party preferred vote, which is obtained through a complicated process of simulating multiple elections using preferences from voters, discarding the least popular option at each run-off until only two candidates remain and it becomes a simple first-past-the-post race. Data on preferences is very difficult to obtain precisely, since it changes from year-to-year quite significantly, usually based on "how-to-vote cards", which are materials handed out by party volunteers at polling places suggesting preference orderings.

This is a difficult problem without an optimal solution. The best solution would be to obtain historical preference profiles for individual seats and use these to simulate each election as a series of run-off elections like the AEC does, performing this a number of times to obtain some sort of probabilistic estimate.