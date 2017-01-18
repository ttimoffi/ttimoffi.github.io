---
layout: post
title: Why I stopped trying to predict the Australian election
image:
  feature: sample-image-11.jpg
  credit: Blue Mountains, New South Wales, 2011
---

It has long been a project of mine to try to build a robust probabilistic model for predicting the outcome of Australian parliamentary elections. I called the project [Emma Chisit](http://www.auselections.com/) and made quite a bit of progress researching a model, obtaining historical poll data, and thinking about the best way to go about building everything. 

I'm stopping the project (for now) for a few reasons:

- lack of time
- insufficient data for the level I want to provide predictions for
- significant uncertainty in particular important seats because of third parties

First, lack of time. It's unfortunate but I always have multiple projects going at once, and a full-time job, and a lot of other things going on, which mean I haven't been able to give this project the focus it needs. My plan is to spend a couple of years going back to basics and building something from scratch which won't have the two other problems I'm about to mention. 

Second, insufficient data. The whole point of differentiation for Emma Chisit was to provide seat-by-seat win probabilities, and seats are just not polled enough in Australia for this to be feasible. I had a couple of ways around this: some demographic-based clustering, the strong transition model and runoff simulations, but I felt like there was a bit too much overfitting going on and that the model wasn't capturing the subtleties and potential chaotic behaviour (chaotic in a mathematical sense) that the preferential voting system in Australian elections can lead to. I think this deserves a rethink from the ground up, and a lot of research, which I'm looking forward to getting into in the next few years to have a highly original, deep and accurate model ready for 2019. 

Finally, third parties. Not to try and shift the blame from my own failure, but the facts that a) the election has the potential to be very close and b) at least four or five lower-house seats have the potential to be won by third parties (the Greens, NXT, One Nation) mean giving an estimate of the makeup of the lower house is difficult (the Senate is impossible). 

Stay tuned for updates about Emma Chisit, which is definitely not dead, and will return for the next election!