---
layout: post
title: Difficulties in forecasting elections in Australia VI - Economic index
image:
  feature: sample-image-12.jpg
  credit: Newtown, New South Wales, 2012

---

In this post I'll explain why it's difficult to economic index to include in our model. In the FiveThirtyEight model, the underlying idea is that a strong economy tends to help the incumbent party. I haven't done a huge amount of research but I would assume this assumption is weaker in Australian parliamentary elections than in American presidential elections. 

The FiveThirtyEight model uses only a few variables to come up with an economic index:

- nonfarm payrolls (this variable measures unemployment)
- personal income
- industrial production
- personal consumption expenditure
- inflation
- forecasted GDP
- stock market

The problem with trying to use this data (or similar data) in Australia is that very few economic variables are published monthly or more frequently by the ABS. This makes using the economic index to guide a forecast built with poll data problematic, as the economic index is operating on a different timescale. I spent a bit of time trying to come up with analoguous variables to these to use, but there just aren't enough variables released on small timescales to make what I would think is a reasonable index. 

For this reason, at least at first, I'm not going to include an economic index in my model, until I can find a way to make a more meaningful one, and until I have done more research to test which particular economic variables are correlated strongly with election results. This again highlights the significant difficulties in aping the FiveThirtyEight model, and is another reason why I'm planning on coming up with a different idea. 