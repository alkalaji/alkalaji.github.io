---
layout: post
title: What drives Airbnb pricing
---

Airbnb serves guests and hosts whether to list a property or find one that matches their need. However, a badly priced property affects both sides negatively. Therefore, it is very important to understand pricing. 

![_config.yml](https://render.fineartamerica.com/images/rendered/share/27275092&domainId=1)

## Introduction

Airbnb revolutionized the industry with its platform by bringing both the hosts and guests directly unto one platform. However, unlike established businesses that work in hospitality, now individuals found themselves as people who running their own business over airbnb. 

Part of a successful business is the right pricing. Therefore, using Airbnb data for Seattle we will try to answer the following business question related to pricing:
- Is there any trend in house prices and availability, and how can it be explained?
- Can we estimate the price of a listing given listing features?
- Does the cancellation policy affect occupancy of the property?

We will tackle those questions one by one. So let's get started!

## Is there any trend in house prices and availability, and how can it be explained?
Pricing always changes especially in sectors like tourism and hospitality, and Airbnb listings are no exception. Factors like seasons, holidays, and so on affect both pricing and demand!

We will start by looking at the average listing prices fo each day and plot over a year's period to see how the prices change:
![_config.yml]({{ site.baseurl }}/images/price-vs-time.png)

We can quicky observe few things from the chart above:
- Pricing over the weekend (in yellow) is abut 5% to 6% higher than usual days throughout the year
- Price over the first half year is generally increasing, whereas it decreases over the second half
- We can see clear spikes near Valentine's day, spring break, summer break, and a small one near new year

However, we cannot look at pricing without looking at availability:
![_config.yml]({{ site.baseurl }}/images/availabile-vs-time.png)

This chart is very interesting because it follows the expected logic but at the same time is counterintuitive. We can observe the following:
- We can see the same dips in availability around seasons as expected
- The small zigzag pattern throughout the chart seems to correspond with weekends
- Counterintuitively, there is an overall positive trend of availability throughout the year

The general trend of increase in availability did not drop the prices, especially during the first half of the year. This could be attributed to other factors that could be revealed upon closer examination of other data sources, such events, or change in demographics in Seattle.

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
