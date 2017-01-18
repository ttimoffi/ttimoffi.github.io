---
layout: post
title: Facebook Kaggle competition, first pass
image:
  feature: sample-image-15.jpg
  credit: Negev Desert, Israel, 2014. 

---

I spent most of today writing small Python scripts to clean and parse the Facebook social circles data from a Kaggle competition. The competition gives egonets, i.e. lists of mutual friends of a user’s Facebook friends, together with a list of features for each friend. The goal is to draw ‘circles’ around various clusters in the friendship graph; around fifty groups of training circles are provided for some users. The competition is passed the new submissions phase, so I’m not going to go very deep into designing models for this competition because I can’t submit to see how well my models are doing (although someone on the forums has written an estimator for the metric Kaggle uses so you can test yourself by bootstrapping the training data). 

My code currently cleans the traning circles data and looks in circles for common features. First we need to parse the circles:

import sys

    def parse_circles(circle_content):
        contents=[]
        for i in range(0,len(circle_content)):
            contents.append(circle_content[i].split(' '))
        for i in range(0,len(contents)):
            for j in range(1,len(contents[i])):
                contents[i][j] = int(contents[i][j])
        return contents

    if __name__ == '__main__':
        if len(sys.argv)<2:
            print "Expected features file.."
            sys.exit(0)

        with open(sys.argv[1]) as f:
            content=f.readlines()

        print parse_circles(content)

And then we write a simple function to look for common features. 

    import json
    import sys
    import circles_parser as cp
    import re

    ##########################################################
    ##
    ## common_features.py
    ##
    ## Code which takes as input user IDs and outputs a list
    ## of features they have in common. Requires a cleaned
    ## feature list.
    ##
    ## 25-10-2014
    ##
    ## run as:
    ## python common_features.py a 'xxx.txt' ID1 ID2 ...
    ## where xxx.txt is a cleaned feature file (a file with
    ## a dictionary corresponding to a user ID on each line)
    ## and a is 0 or 1 depending on whether to consider
    ## gender as a feature or not.
    ##
    ## or as:
    ## python common
    ##
    ##########################################################

    temp_list=[]
    with open('featureList.txt') as f:
        for line in f:
            temp_list.append(line)

    feature_list=[]

    for i in range(0,len(temp_list)):
        feature_list.append(re.sub('([\D])([;])([\D])','\g<1>_\g<3>',temp_list[i])[:-1])

    ##

    def find_common_features(list_of_ids,features_dict,ignore_gender=True):

        these_features_list=[]

        for i in range(0,len(features_dict)):
            if (features_dict[i][u'id'] in list_of_ids):
                these_features_list.append(features_dict[i])

        if ignore_gender:
            for i in range(0,len(these_features_list)):
                try:
                    del these_features_list[i][u'gender']
                except KeyError:
                    pass

        intersection_dict=these_features_list[0]
        for i in range(0,len(these_features_list)-1):
            intersection_dict={x:intersection_dict[x] for x in intersection_dict if x in these_features_list[i+1]}

        common_features={}

        for key in intersection_dict:
            keys=[]
            for d in these_features_list:
                keys.append(d[key])
            if all(x==keys[0] for x in keys):
                common_features[key]=keys[0]

        return common_features

    def feature_count(circle,features_dict):

        these_features_list=[]

        for i in circle[1:]:
            these_features_list.append(features_dict[i])

        feature_counts=[]

        for i in range(0,len(feature_list)):
            feature_counts.append([feature_list[i]])
            for i in circle[1:]:

    gender=True

    if __name__ == '__main__':

        our_features_dict=[]

        with open(sys.argv[1]) as input_file:
            for line in input_file:
                our_features_dict.append(json.loads(line))

        if int(sys.argv[2])==0:
            gender=False

        if len(sys.argv[3].split('.'))==1:    # we've been given a small list of
            id_str_list=sys.argv[1:]      # user IDs as integers
            id_list=[]
            for entry in id_str_list:
                id_list.append(int(entry))
            print find_common_features(id_list,our_features_dict,gender)

        if len(sys.argv[3].split('.'))==2:    # we've been given a .circle file
            with open(sys.argv[3]) as f:
                content=f.readlines()

            circles_list=cp.parse_circles(content)

            print circles_list[0]

            for i in range(0,len(circles_list)):
                print 'Circle of size ', len(circles_list[i]),', features in common: ', find_common_features(circles_list[i][1:],our_features_dict,gender)

As I expected most circles have no features in common, and some small circles have gender, location or work in common. No other features appeared as common. I also wrote a script using networkx to split the graphs into connected components, an obvious benchmark, and read someone else’s script using networkx to split the graph into ‘k-clique communities’, a small generalisation of connected components. If I was to pursue this problem, which I might come back to a bit, I would be interested in learning some more sophisticated machine learning clustering algorithms for graphable data, to explore the networkx package again, and to actually think hard about the features which are relevant to define social circles, beyond my simple-minded features-in-common approach (which doesn’t really allow for drawing meaningful new circles).