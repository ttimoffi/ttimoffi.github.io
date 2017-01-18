---
layout: post
title: Repetition index
image:
  feature: sample-image-26.jpg
  credit: Prague, Czech Republic, 2013

---

This post is based on a question I was asked in a job interview. I had trouble thinking properly in the interview, so I wrote up this code to answer the question afterwards. 

Suppose we run a website (maybe an online newspaper) with a number of different articles, which we call sources. We have developed an in-house advertising system which places an ad, called the target, at the bottom of each source page. We recieve a table containing the following columns:

`date | source | target | count`

That is, for a given day, the number of people who browsed each source and which target they saw. For example, maybe on April 9, there were fifteen users who browsed source 1 and saw target a; this would be represented as the row 

`9/4 | 1 | a | 15`

We are tasked with developing a "repetition index" for a given day, that is, some quantifiable and subjective measure of a user's feeling that they are repeatedly seeing the same targets. 

Let's model user behaviour on the website as follows:

- the probability of a user visiting a given source on a site can be estimated using the count data
- a user is smart enough not to visit the same source twice on the same site on the same day

(An estimate of) the probability that a user navigates to source s which recommends target t is easily computable using the count data. So is the probability that a user then navigates to another source that is not s and sees target u. We can then look at all possible "paths" that a user could take, first visiting one site, and then a different one, and compute the probability that they see the same reccommendation on both sources. 

Using Python:

    def ProbOfPath(table,s,t):

    ## Estimates the probability of a user browsing to 
    ## source s recommending target t, using 
    ## the count data. 

        sum = 0
        total = 0
        for i in range(0,len(table)):
            total = total + int(table['count'][i])
            if table['source'][i] == s and table['target'][i] == t:
                sum = sum + int(table['count'][i])

        return float(sum)/float(total)

    def CondProbOfPath(table,r,s,t):

    ## Estimates the probability of a user browsing to
    ## source s recommending target t, having
    ## already browsed source r, using the count data. 

        sum = 0
        total = 0
        for i in range(0,len(table)):
            if table['source'][i] != r:
                total = total + int(table['count'][i])
                if table['source'][i] == s and table['target'][i] ==t:
                    sum = sum + int(table['count'][i])

        return float(sum)/float(total)

    def RepetitionIndex(table):

        sources = list(set(table['source']))
        targets = list(set(table['target']))

        total_count = 0
        for i in range(0,len(table)):
            total_count = total_count + int(table['count'][i])

        combinations = {}

        for i in range(0,len(table)):
            if int(table['count'][i]) != 0:
                comb = repr([int(table['source'][i]), int(table['target'][i])])
                combinations[comb] = float(int(table['count'][i]))/float(total_count)

        paths = {}

        for first_combination in combinations.keys():
            for second_combination in combinations.keys():
                if first_combination[1] != second_combination[1]:
                    comb = repr([first_combination, second_combination])
                    paths[comb] = ProbOfPath(table,int(first_combination[1]), int(first_combination[4]))*CondProbOfPath(table,int(first_combination[1]), int(second_combination[1]),int(second_combination[4]))

        count_list = []

        prob_of_same = 0

        for path in paths.keys():
            if int(path[6]) == int(path[16]):
                prob_of_same = prob_of_same + paths[path]
        return np.round(prob_of_same,3)

A few comments:

- This model is maybe not the most perfect measure of repetition; in particular it is not invariant under one particular source having its traffic increased. Notice that this does not violate the assumptions of the model, since if traffic to source 1 increases, and source 1 recommends target 2, then the total number of users seeing target 2 will inevitably increase. 
- The code above is really hacky and dumb; in particular it's going to take a very long time to compute things for real data sets. It should be redesigned in a smarter, object-oriented way. 
- This model gives an estimate of a fairly precise thing: the probability that a user sees the same target recommended twice. This measure is presumably highly correlated with a user's sense of "repetitiveness" but is not the only possible measure. 
- A better model would compute longer "paths" through the site and look at the probability that a single target was seen an "unusually high" number of times. 
