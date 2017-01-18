---
layout: post
title: Aggregating Israeli opinion polls I - Poll reliability
image:
  feature: sample-image-22.jpg
  credit: Royal National Park, New South Wales, 2014

---

The Israeli electoral system is completely different to the Australian system. 120 representatives are elected to the Knesset from a single at-large electorate. Members must be registered to a party; parties submit lists of candidates in an order they want them to be elected, candidates are then elected to the Knesset from these lists based on a proportion of votes received by each party, provided the vote is greater than the electoral threshhold (currently 3.25%, a significant increase from the previous election). 

It is difficult to provide a probabilistic model of what the Israeli government will look like after the election, even with the significant amount of polling that exists, because Israel's proportional representation system has meant, like many European countries in the post-war period, majority governments never occur. Rather, the election is just the first phase of a process that is followed by political wrangling and deals behind closed doors to form a coalition government (and these are often very unstable: the current election is more than twelve months early because of the breakdown of a previous coalition). 

In these two posts I just want to come up with a simple poll aggregator that adjusts for accuracy in previous contests. There's a whole bunch of opinion polls floating around but most aggregators (like the one on HaAretz) just seem to take a simple average (although I imagine there's plenty of sites in Hebrew that I can't read yet). For this first step, I want to look at the final poll all the polling firms did before the 2013, 2009 and 2006  elections and come up with an accuracy score for each polling firm which will then translate into a weight in the aggregator. 

I looked at the Wikipedia pages for each election, which contain a lot of opinion polling information, and put together lists of final polls before the respective elections (historical_polls) for lots of different polling firms. I also needed the results of each election (results). 

    import pandas as pd
    from pandas import DataFrame

    historical_polls = pd.read_csv('israeli_pre_election_polls.csv',delimiter=',')
    results = pd.read_csv('israeli_results.csv',delimiter=',')
    results = results.set_index('year')
    historical_polls = historical_polls.sort('pollster')

The following code produces a fairly simple weighting mechanism, where a pollster with average error receives a weight of 1; below-average pollsters receive less and above-average more. 

    parties = ['Kadima', 'Likud', 'Yis Bet', 'Labor', 'Shas', 'UTJ', 'Hadash', 'Balad', 'Meretz', 'Yesh', 'Otzma', 'Am', 'Hatnuah', 'NU', 'Greens', 'Gil']

    final_frame = pd.DataFrame(columns = historical_polls['pollster'].unique(),index=results.index)

    for firm in historical_polls['pollster'].unique():
        sum = 0
        for party in parties:
            sum = sum + abs(results[party] - historical_polls[historical_polls['pollster']==firm].set_index('year')[party])*historical_polls[historical_polls['pollster']==firm].set_index('year')[party]/120
            days = historical_polls[historical_polls['pollster']==firm].set_index('year')['days_before']
        final_frame[firm] = sum

    for firm in historical_polls['pollster'].unique():
        means = final_frame.mean(axis=1)
        final_frame[firm] = abs(final_frame[firm] - means)

    total_error = pd.DataFrame(final_frame.mean(axis=0))
    av_error = total_error.mean()[0]
    print av_error
    df = 1- (final_frame.mean(axis=0) - av_error)
    df.to_csv('poll_weights.csv',sep=',')

This produces the following output:

    Dahaf               1.412384
    Dialog              1.303897
    Geocartography      1.352617
    Jpost               1.425932
    Maagar Mochot       1.137197
    Midgam              1.271375
    New Wave            0.831736
    Panels              1.176537
    Shvakim Panorama    0.585856
    Smith               0.635624
    Sof Hashavua        0.470412
    Teleseker           0.396433
    dtype: float64
    [Finished in 1.2s]

In the next post we'll incorporate these weights into a poll aggregator. 
