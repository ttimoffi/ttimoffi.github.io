---
layout: post
title: Sentiment analysis of Q&A tweets I - Sentiment analysis using out-of-the-box tools
image:
  feature: sample-image-27.jpg
  credit: Amsterdam, The Netherlands, 2012
---

For a while I've been keen to learn how to use the Twitter API to scrape tweets. In this post I'll do some basic sentiment analysis on tweets I scraped during the Monday 20/4 airing of QandA, an Australian current affairs panel show which has become infamous in Australia for its lively Twitter feed. 

The panel for this program consists of five people, together with the host Tony Jones. The panel this episode consisted of 
 
- Derryn Hinch, a journalist and former radio personality who is famous for his outspoken views on child molestation rings (he has often defied court orders against naming child abusers)
- Andrew Robb, a member of the current conservative government who is the minister for trade and investment
- Anna Burke, a member of the centre-left opposition who used to be the house speaker
- Dave Hughes, a popular comedian
- Jane Burns, CEO of a research group that studies how technology affects the lives of young people 

As the program is political in nature, the ABC usually provides a breakdown of the political identifications of their audience, and attempts to make this breakdown as similar as possible to the true voting intentions of the Australian public.

I wanted to try and assess the political leanings of the Twitter users using the hashtag #QandA during the episode's broadcast. This required a number of steps.

**1. Scraping Twitter for tweet data**

Twitter has a very usable API and there's a great Python package called `tweepy` that allows you to automate everything using a Python script. I fiddled around with the default tweepy script to scrape the Twitter stream live for a certain search term, using the term `QandA`, and to write the output to a text file. The free API is limited to only providing 1% of the Firehose (Twitter's in house term for the live feed of all tweets), but since Q&A only get around 20 000 tweets per episode, there's no way this data is going to exceed 1% of the 6000 tweets per second in the Firehose. So I set the script to run from 9.20PM to 11:00PM AEST on Monday April 20 and saved only the body text of all tweets using the hashtag (one interesting thing I learned is what a big-name hashtag looks like after the mainstream traffic has disappeared. When the show isn't broadcasting, #QandA is a whole bunch of hideous homophobic propaganda). 

**2. Devising sentiment analysis**

The very basic first-pass analysis I want to do is a little different to the usual textbook Twitter sentiment analysis problem of classifying a tweet as happy or sad. Classifying a tweet as right or left is quite hard, so rather than that I want to to do the following process:
- identify if a tweet contains a single pertinent political keyword ('Tony Abbott', 'Coalition', 'ALP', etc. etc.) which has a particular alignment on the political spectrum
- assess the sentiment of the other words in the tweet
- come up with a corresponding political ranking (right or left will do; it's hard to quantify beyond that)

For example, the tweet 

`I don't understand how the Coalition can be so cruel towards asylum seekers #QandA`

would be rated as left-wing, since it contains the keyword `Coalition` and the negative-sentiment marker `cruel`. 

This is a pretty crude model, but let's use it for a first attempt. So of all the tweets scraped during the broadcast, I only want to look at

- those containing a mention of a left- or right-wing entity
- those which have terms in my sentiments dictionary (obtained online and built on Twitter data, but not specifically Australian or political, so it won't be perfect). 

The code looks like this:

    import pandas as pd
    from pandas import DataFrame
    import math
    import csv
    import numpy as np

    frame = pd.read_csv('twitter_sentiment_list.csv',skiprows=0,delim_whitespace=False,header=0,names=['term','pos','neg'])
    frame.index = frame['term']

    with open('left_list.txt', 'rb') as f:
        reader = csv.reader(f)
        left_list = list(reader)[0]

    with open('right_list.txt', 'rb') as f:
        reader = csv.reader(f)
        right_list = list(reader)[0]

    with open('tweetdump.txt') as f:
        tweets = f.readlines()

    tweet_total = 0
    tweets_analysed = 0
    left_tweets = 0
    right_tweets = 0

    for tweet in tweets:

        tweet_total += 1

        tweet_words = tweet.split(' ')

        right_count = 0
        left_count = 0

        keywords = []

        for right_term in right_list:
            if right_term in tweet_words:
                keywords.append(right_term)
                right_count += 1
        for left_term in left_list:
            if left_term in tweet_words:
                keywords.append(left_term)
                left_count += 1

        if (left_count >= 1 and right_count == 0) or (left_count == 0 and right_count >= 1):

            tweets_analysed +=1

            pos_sentiment = 0
            neg_sentiment = 0

            for word in tweet_words:
                if word in frame['term'].values:
                    pos_sentiment = pos_sentiment + frame['pos']['term']
                    neg_sentiment = neg_sentiment + frame['neg']['term']
            prob_happy = np.reciprocal(np.exp(neg_sentiment - pos_sentiment)+1)
            prob_sad = 1-prob_happy
            if left_count >= 1:
                print tweet, '| mentions left entity with keywords ' + ''.join(keywords) + ', '+str(np.round(prob_happy,3)) +'% of being positive'
                if prob_happy > 0.5:
                    left_tweets +=1
                elif prob_happy <0.5:
                    right_tweets +=1
            else:
                print tweet, '| mentions right entity with keywords ' +''.join(keywords) + ', ' +str(np.round(prob_happy,3)) +'% of being positive'
                if prob_happy >0.5:
                    right_tweets +=1
                elif prob_happy <0.5:
                    left_tweets +=1


    print 'Total tweets: ' + str(tweet_total)
    print 'Tweets analysed: ' + str(tweets_analysed) + ' (' +str(np.round(100*float(tweets_analysed)/float(tweet_total),1))+'%)'
    print 'Left tweets: ' +str(left_tweets) + ' (' +str(np.round(100*float(left_tweets)/float(tweets_analysed),1))+'%)'
    print 'Right tweets: ' +str(right_tweets) + ' (' +str(np.round(100*float(right_tweets)/float(tweets_analysed),1))+'%)'

And here's the results:

    Total tweets: 21952
    Tweets analysed: 2945 (13.4%)
    Left tweets: 463 (15.7%)
    Right tweets: 1793 (60.9%)
    [Finished in 14.7s]

**3. Analysis of output**

Some analysis of this then. Firstly, we're only looking at 13.4% of the tweets with this basic model. And of these tweets, it's classifying only 16% of them as being from left-wing users (which doesn't seem quite right to me). 

    @qanda Robb ' meds are obviously working well.  He is now as a LNP minister unwilling to support the most vulnerable and at risk
    | mentions right entity with keywords Robb, 0.815% of being positive
    | sentiment derived from words medsobviouslynow
    Love how #QandA only let Anna Burke respond directly to #eastwestlink
    | mentions left entity with keywords BurkeAnna, 0.729% of being positive
    | sentiment derived from words letrespond
    Abbott called the Vic state election a referendum on east west link- and Daniel Won #qanda
    | mentions right entity with keywords Abbott, 0.729% of being positive
    | sentiment derived from words eastwest
    Bloody Greens and their bloody communist public transport! Where will it end? #qanda
    | mentions left entity with keywords Greens, 0.729% of being positive
    | sentiment derived from words bloodypublic
    Great topics on @QandA tonight! #Labour MP Anna Burke is especially #entertaining
    | mentions left entity with keywords BurkeAnna, 0.729% of being positive
    | sentiment derived from words topicsespecially
    #qanda Robb suggests that his Govt needs to be the one to calm the fear brought about by terrorism &amp; radicalisation LOL @QandA
    | mentions right entity with keywords Robb, 0.815% of being positive
    | sentiment derived from words needscalmfear

This comes down really to the difficulties of performing sentiment analysis on tweets. Seeing as tweets are only 140 characters, it's very difficult to get a proper picture of the sentiment. Of the six tweets I posted above:

- the model gets the first one exactly right. There's not enough information there to classify the sentiment, and it rightly identifies the entity. 
- the second one is a bit harder to classify because of its sarcastic tone but the model gets it right using the keywords
- the third tweet is misclassifed (it's very hard for a model to get this right as it's a sarcastic comment about regional politics in the state of Victoria) and the model is using irrelevant words to get the sentiment
- the fourth tweet is misclassified by sentiment as well
- the fifth tweet is classified exactly right
- the final tweet is again misclassified because the sentiment analysis isn't accurate enough

**4. Next step**

Because the main issue is the accuracy of the sentiment analysis (the left-right entity recognition is basic but works well enough), I thought I'd fix this by developing my own sentiment dictionary. I want to hand-label as many tweets as I can as positive or negative (or irrelevant) and then use this data to build a sentiment dictionary for next week's program's tweets. 
