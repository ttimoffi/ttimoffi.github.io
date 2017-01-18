---
layout: post
title: Difficulties in forecasting elections in Australia II - Constituency sizes
image:
  feature: sample-image-8.jpg
  credit: Blue Mountains, New South Wales, 2014

---


It is worthwhile noting that FiveThirtyEight does **not** attempt to forecast US congressional elections on a district-by-district basis (instead providing a probabilistic model for the Congressional makeup as a whole). This however is the sort of analysis one would expect in Australian elections to provide an analog to Silver's presidential forecasts. Except on an occasional, one-off basis, individual seats are **never** polled in Australia, so seat-by-seat data is non-existent (and when it is available, is too sporadic to be worthwhile).

The most robust and accurate opinion polling is conducted on a **countrywide** basis; often these countrywide results are broken down into states, but never into electoral districts. There should be a way to use clustering and nearest-neighbour analysis to obtain extrapolated polling estimates for individual districts. Indeed, FiveThirtyEight.com does a similar thing, using polls in "similar" states (where clustering is used to obtain a measure of similarity) as proxies for polls. For example, if a poll is released in New Mexico, and New Mexico has quite similar demographic features to Arkansas, the New Mexican poll probably has some bearing on trends in Arkansas. 

In Australia, my idea is to turn each national poll into one hundred and fifty simulated seat polls, based on trends and demographic data, and then to perform the usual aggregation and regression on these polls. 