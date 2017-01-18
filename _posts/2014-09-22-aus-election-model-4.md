---
layout: post
title: Difficulties in forecasting elections in Australia IV - Redistributions
image:
  feature: sample-image-20.jpg
  credit: Black Mountain, Australian Capital Territory, 2012. 

---


As a fantastic bulwark against gerrymandering, and in a democratic statement that should be the envy of democracies worldwide, electoral districts in Australia are not fixed, but have boundaries which change according to shifting demographics as measured by census data. This makes precise comparisons between historical elections extremely difficult on a seat-by-seat basis, as many seats cease to exist between elections, or have become so distorted from their original boundaries that there is no meaning in comparing them.

If possible, I would like to obtain historical election data for, and extrapolate polling data to, polling places rather than just electoral districts. Since it is always entire polling places that are reassigned to new or different districts, this will allow a more accurate calculation of the model on redistributed seats (Antony Green regularly performs the explicit calculation of "redoing" elections on new boundaries: a model would need to be built, preferably using demographic data from the census, to project opinion polling data onto polling places).

It should be clear that this would be an extremely lengthy and time-consuming process not without significant complications:

- there are [nearly 8 000](http://www.aec.gov.au/about_aec/cea-notices/election-pp.htm) polling places in the country for federal elections
- census data can be obtained grouped by federal district (and in some states, by state district), but **not** by polling place
- I'm not sure if state electoral commissions publish results by polling place 

It seems to me that, given the infeasibility of performing the above task, a better idea would be to perform some sort of correction for redistributions similar to what Antony Green does when calculating "nominal swings" (his figures get used by all media outlets all over the country). 
