---
layout: post
title:  "Teen Pregnancy - Identifying High-Risk Areas Through Machine Learning"
date:   2017-01-24 16:00:00
categories: machine_learning
tags: machine_learning public_health d3.js
image: /images/2016-01-23-Teen-Pregnancy/teen_preg_icon.png
---

## Overview

**The good news**--from 2007 to 2014, teen pregnancy rates within the U.S. (defined as live births per 1,000 births to women 15-19 years of age) have fallen from [41.5 to 24.2](https://www.cdc.gov/teenpregnancy/about/), a reduction of 42%.  

**The bad news**--[relative to other developed Western countries](http://www.dailymail.co.uk/news/article-2794234/uk-highest-rate-teen-pregnancies-western-europe-despite-25-fall-decade.html) the U.S. is still exceedingly high, nearly double the rates of the European Union as a whole. Though the map below is as of 2009, the relative disparities between countries persist as of more recent data:  

### Global Teen Pregnancy Rates, 2000-2009

![Global Teen Pregnancy Rates, 2009](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-23-Teen-Pregnancy/global_birth_rates.jpg?raw=true)

The social costs of teen pregnancy are [well-publicized](https://www.hhs.gov/ash/oah/adolescent-health-topics/reproductive-health/teen-pregnancy/index.html#): teen mothers are more likely to drop out of school, rely on public assistance, and live below the poverty line, while the children of teen mothers are more likely to have health problems, commit crimes, and do poorly in school relative to peers.

Less well-known are the explicit economic costs: over [$9 billion](http://thenationalcampaign.org/sites/default/files/resource-primary-download/counting-it-up-key-data-2013-update.pdf) in aggregate costs to U.S. taxpayers annually (as of 2010, costs are healthcare, welfare, and incarceration), or approximately $1,600 per child per year from birth until age 15.

In that the government only spends ~$200 million a year on prevention ([estimated $179.5 million](http://www.npr.org/sections/itsallpolitics/2015/08/05/429641062/fact-check-how-does-planned-parenthood-spend-that-government-money)[^1] on Planned Parenthood contraceptive services, [$1.5 million](https://www.hhs.gov/sites/default/files/budget/fy2016/fy2016-general-departmental-budget-justification.pdf) on the Office of Adolescent Health, and [$17.2 million](https://www.cdc.gov/budget/documents/fy2017/fy-2017-cdc-congressional-justification.pdf) on the CDC's Teen Pregnancy Prevention programme), it is clear that taxpayer money is not being spent as effectively as it could.

Absent a complete realignment of spending priorities, one way to stem the cost is to target areas most at-risk for elevated teen pregnancy rates--working under the assumption that every dollar spent in high-risk areas will have a greater impact overall than those spent in lower-risk areas. To do this, one would have to determine what features are shared by areas with elevated teen pregnancy rates to track and target. Rather than use surveys or anecdotal evidence, one could use clustering and classification machine learning algorithms to determine which features most closely track teen pregnancy rates, for a more quantitative and robust targeting methodology.

## Results

Of the 80+ features initially collected to identify areas of high risk, the 11 most indicative of a county having elevated teen pregnancy rates are a mix of correlative and causative factors:  

* Child poverty rate  
* Incidents of Chlamydia per 1,000  
* Share of population who are religious adherents[^2]  
* High school graduation rate
* Share of women without health insurance
* Share of households with a single parent
* Numer of religious congregations per 10,000 people
* Binge drinking rate
* Smoking rate
* Obesity rate
* Violent crime rate

In the below visualization one can see the teen pregnancy rate by county, and then click through key features to see the overlap:  

[![Teen Pregnancy Choropleth](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-23-Teen-Pregnancy/teen_preg_map.png?raw=true)](https://bl.ocks.org/ramohse/raw/2fc980850631f9392267cd60399f6a66/ "Teen Pregnancy Choropleth")

#### Recommendations Based on the Analysis

Based on the analysis and visualization, it is clear that the areas most at risk are those that would anecdotally have strong religious and/or political aversion to more pragmatic and liberal actions to prevent teen pregnancy (i.e. expansive sex education in public schools, free contraceptives, birth control prescriptions for teens without parental consent).  

As such, rather than fighting an uphill and costly battle for explicit endorsement in target areas, a two-pronged strategy at the state and grassroots levels for benign disregard might be more effective--i.e. allocating a portion of Planned Parenthood's [lobbying budget](https://www.opensecrets.org/lobby/clientsum.php?id=D000000591) towards state legislatures for the tacit approval to set up condom distribution booths at places where teens congregate (malls, parks, concerts), billboards advertising websites with information, and other low-profile means of distributing resources. 
 
Further research into the efficacy of such an effort would have to be done, but as a first step this seems reasonable and attainable.


## Methodology & Data

* The above analysis was done using a combination of clustering algorthms and classification algorithms[^3], with simple logistic regression performing the best in terms of both accuracy (83%) and f1 (72%) scores.

* All data used was as of 2010, the last national census and religious census, with voting data by county as of the 2008 presidential election.

* For the purposes of analysis, "elevated" teen pregnancy rates were classified as anything one half of one standard deviation above the national mean--in this case, 56 live teen births per 1,000 live births. 

* Over 80 features were initially used, collected via .csv downloads and Selenium web-scraping from the [County Health Rankings](http://www.countyhealthrankings.org/rankings/data), the [Guttmacher Institute](https://data.guttmacher.org/regions), the [Association of Statisticians of American Religious Bodies](http://www.rcms2010.org/), county-level [voting data](https://catalog.data.gov/dataset/2008-presidential-general-election-county-results-direct-download), and the [CDC](https://www.cdc.gov/teenpregnancy/). Five columns of random noise were added to "gut-check" feature importance--algorithms were run on all features, with features whose importance (as ranked by the random forest regressor) was below random noise were dropped for the final evaluation.  

* The visualization was made with d3.js. Rather than link the data directly to the choropleth (which would require generating a new map for each color scheme for each feature), the data were bucketed into nine groups (noniles) mapped to RGB codes of color schemes. This way rather than changing the underlying data, each button changes which colors go to each county--an easier way to change color schemes in d3.

Code for all of the above can be seen on my [github page](https://github.com/ramohse/Metis_Teen_Pregnancy).



#### Notes

[^1]: $528 million government allocation to Planned Parenthood times the ~34% of Planned Parenthood budget spent on contraceptive services  
[^2]: Adherents [defined](http://www.rcms2010.org/images/2010_US_Religion_Census_Appendix_A.pdf) as all persons (adults and children) participating in the life of a parish, regular or occasional attendees  
[^3]: Algorithms used: logistic regression, random forest, gradient-boosted trees, k-nearest neighbors, decision trees, and Gaussian naive Bayes