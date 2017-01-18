---
layout: post
title: Writing ETLs and data science
image:
  feature: sample-image-43.jpg
  credit: New Forest, Hampshire, England, 2016
---

I recently read [this](http://www.kdnuggets.com/2016/03/engineers-shouldnt-write-etl.html) post about the benefits of data scientists writing production ETL code. For the uninitiated, ETL stands for Extract Transform Load and refers to any code used to manipulate, transform and move data from one place to another. In moden data pipelines, raw data usually arrives into a single large "data lake", and ETL processes extract it, transform it, aggregate it and load it into other databases or locations. 

The article was released at an auspicious time for me, as I've been spending quite a bit of my time at work lately writing ETL code (in Python). My code consists of a number of scripts which pull raw data from a large database, process it and load it into another database which is used in production by one of our data products. 

As the article mentions in some detail, this is not a usual scenario for a data scientist. Particularly in large corporations (ours is not especially large), there is usually a rigid demarcation between the roles and responsibilities of a data scientist and those of a data engineer. I think the end-to-end approach advocated by the article is admirable, and works very well for me, although I'm not sure it would work for everyone and in every workplace, particularly a larger one with many experienced and senior data scientists. 

Writing and maintaining the ETL code means I am responsible for the *full* length of the pipeline providing data to the product: I know the schema and the structure of the data lake inside out, I know the processes that extract the data, have written the code which transforms and aggregates it, and know the schema and structure of the production database on the other end. The aggregation algorithms in the ETL (which in this particular project were non-trivial and required a good deal of optimisation and thought) are probably the only traditional "data science" part of this particular project, but doing the entire thing myself taught me a whole lot of invaluable things about data programming, Python, pandas, as well as more technical things which would usually be in the domain of DevOps (like scheduling jobs, setting up monitoring and alerts, deployment and staging etc). 

At the end of the day, this comes down to personal preference, and probably a little bit to ego as well. Data science has become such a buzzword in the last five years (although this is already starting to fade a little bit) that many data scientists will see the other parts of working with data, which are not very high-level and cerebral, as "beneath them". I think this is counterproductive and arrogant, and I think data scientists and the companies they work for could definitely benefit from considering a more end-to-end approach as advocated by this article. It certainly taught me a lot. 

