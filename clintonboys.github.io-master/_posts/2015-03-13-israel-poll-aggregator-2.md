---
layout: post
title: Aggregating Israeli opinion polls II - The Aggregator 
image:
  feature: sample-image-14.jpg
  credit: Agmon HaHula, Israel, 2014

---

Having produced a series of reliability weightings, I scraped Wikipedia for opinion polls for this year's election in the last thirty days (following methodology from 538 where polls beyond this period lose their utility) (israeli_2015_polls.csv). We have to combine the reliability weightings with a recency weighting: more recent polls are weighted more highly. We're going to use the brilliant datetime package to sort out all the dates. 

    import datetime
    import numpy as np
    import pandas as pd

    polls = pd.read_table('israeli_2015_polls.csv',delimiter=',')
    polls['date'] = pd.to_datetime(polls['date'],dayfirst=True)
    poll_ratings = pd.read_csv('poll_weights.csv',delimiter=',')
    poll_ratings.columns = ['pollster','rating']

    today = datetime.datetime(2015,3,12)

The following function weights polls in the average according to their recency. 

    def exp_decay(days):
        days = getattr(days,"days",days)
        return .5 ** (days/30.)

We now inner-merge the poll data with the pollster reliability ratings to get a single table, and add in the recency weights to each poll (I briefly forgot the nice pandas way to add columns like this and did it the dumb way with a list). 

    polls = polls.merge(poll_ratings, how="inner", on="pollster")

    recency_weights = []
    for i in range(0,len(polls['date'])):
        recency_weights.append(exp_decay(pd.to_datetime(today,dayfirst=True)-polls['date'][i]))

    polls['recency_weights']=recency_weights

Now we put everything together and add up the average to get a nice poll aggregate:

    parties = ["L", "YB", "YA", "ZC", "BY", "S", "UTJ", "M", "A", "Y", "K"]

    for party in parties:
        print party+": " + str(np.round(np.sum(polls['rating']*polls['recency_weights']*polls[party]/(polls['rating']*polls['recency_weights']).sum()),2))

This produced the following output on March 13, four days before the election with the inclusion of the final polls before the four-day embargo:

    L: 22.66
    YB: 5.56
    YA: 12.04
    ZC: 23.76
    BY: 12.05
    S: 6.87
    UTJ: 6.77
    M: 5.24
    A: 12.49
    Y: 4.2
    K: 8.49
    [Finished in 1.0s]

There's a few other intricacies I'd like to add to the model in the coming days if I have time:

- performing the aggregate on votes rather than seats and then assigning seats using the Knesset method, including surplus-vote agreements
- adding in weighting for sample sizes (these are very hard to come by for Israeli polls which doesn't sit well with me)
- adjusting for trends to give a forecast for election day (the current model gives a snapshot of what would happen if the election were held today)
- change to a probabilistic model which simulates 10 000 elections and gives probabilities of winning
- add some sort of analysis of Phase 2 of the electoral process, including potential coalitions etc (this is fraught with difficulty however)

**UPDATE** (after the election): My model, like all the polls it aggregated, did not perform very well. Even adding the trend adjustment could not have anticipated the morally questionable tactics used by Netanyahu to increase his seat count in the Knesset. The main lesson I took out of this, other than learning how to build a poll aggregator, is how difficult Israeli elections are to predict. It should be mentioned that the two main sources of error are my estimates of Likud and Habayit Hayehudi; without Netanyahu's last-minute cannibalisation of these votes, my model would have been fairly accurate. 
