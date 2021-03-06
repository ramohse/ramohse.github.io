---
layout: post
title:  "Subway Turnstile Analysis - Metis Project Benson"
date:   2016-09-26 16:52:07
categories: metis python pandas
tags: subway pandas python
image: /images/2015-09-26-Benson/Benson_subway_map_icon.jpg
---
Week 1, Project 1 of Metis data science boot camp is *down*! 

![benson gif](http://i.giphy.com/Ir7eNxjIMqy6k.gif)

True to form of an experience labeled “boot camp”, we went from 0 to 55 mph<sup>1</sup> in the span of a week, via a group data science project ("Project Benson") leveraging MTA turnstile data to help a fictional client optimize deployment of street teams to solicit email addresses. It was a great introduction into the course, our classmates, and the sorts of analytical tools and approaches we’ll employ in the coming 12 weeks. So without further ado…

# The Data

We used data of turnstile entries and exits tracked by the [NYC MTA](http://web.mta.info/developers/turnstile.html), presented in large, relatively clean comma-separated text files. I say “relatively”, as there were a handful of turnstiles that had clearly broken, yielding very high or negative or straight-out weird entry numbers. We removed the outliers (readings >10,000 or negative readings), and began by calculating total entries by day and time at the total station level. Working under the assumption that more station entry/exit volume = more opportunities to solicit emails, we focused on the top 15 stations by average volume, as well as other typically-less-busy stations that might have periods of heightened activity. [chart below]

### The Top 15 Stations
---
![Top 15 Stations](https://github.com/ramohse/ramohse.github.io/blob/master/images/2015-09-26-Benson/Benson_Top.png?raw=true)
---

# The Findings

We found that the top 15 stations were consistently the busiest, but that volume tended to drop off dramatically on the weekends. There were a handful of stations that were more busy on the weekends (Barclays, Canal St) and could be good opportunities to engage with people who aren’t in a rush to get to work/home.  

### Top Stations by Weekday/Weekend--Activity Drops Off on the Weekend, Barclays and Bedford Pick Up
---
![Top Stations by Weekday/Weekend](https://github.com/ramohse/ramohse.github.io/blob/master/images/Benson_weekday.png?raw=true)

In terms of the time of day, the weekdays largely exhibited patterns one would expect (higher activity during rush hours), but there were a few stations (like Union Sq) that were busier during midday—another possible opportunity.  

### Weekday Patterns by Time of Day
---
![Weekday Patterns by Time of Day](https://github.com/ramohse/ramohse.github.io/blob/master/images/2015-09-26-Benson/Benson_weekday_hour.png?raw=true)


On the weekends, activity was much lighter in the mornings, and tended to increase through the day, peaking between 8pm-midnight:  

### Weekend Patterns by Time of Day
---
![Weekend Patterns by Time of Day](https://github.com/ramohse/ramohse.github.io/blob/master/images/2015-09-26-Benson/Benson_weekend_hour.png?raw=true)

As such, we recommended a consistent presence in midtown (where a bulk of the busiest stations are), shifting focus on the weekends to the more “social” stops, and to keep smaller stations with unique periods of activity in mind, to engage with non-commuters.

# Conclusion

In all, a great project. I learned a ton and got my first real taste of what the next 11 weeks will be like. Looking forward to it!



<sup>1</sup>[Top speed][id] of a New York City subway car  

[id]: http://www.nyctransitforums.com/forums/topic/16621-top-speed-limit/
