---
layout: post
title: What drives Airbnb pricing
---

Airbnb serves guests and hosts whether to list a property or find one that matches their need. However, a badly priced property affects both sides negatively. Therefore, it is very important to understand pricing. 

![_config.yml](https://render.fineartamerica.com/images/rendered/share/27275092&domainId=1)

## Introduction

Airbnb revolutionized the industry with its platform by bringing both the hosts and guests directly into one platform. However, unlike established businesses that work in hospitality, now individuals found themselves as people who are running their own business over airbnb. 

Part of a successful business is the right pricing. Therefore, using Airbnb data for Seattle we will try to answer the following business question related to pricing:
- Is there any trend in house prices and availability, and how can it be explained?
- Can we estimate the price of a listing given listing features?
- What are the factors determining the price of a listing?

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

![_config.yml]({{ site.baseurl }}/images/available-vs-time.png)

This chart is very interesting because it follows the expected logic but at the same time is counterintuitive. We can observe the following:
- We can see the same dips in availability around seasons as expected
- The small zigzag pattern throughout the chart seems to correspond with weekends
- Counterintuitively, there is an overall positive trend of availability throughout the year

The general trend of increase in availability did not drop the prices, especially during the first half of the year. This could be attributed to other factors that could be revealed upon closer examination of other data sources, such events, or change in demographics in Seattle.

## Can we estimate the price of a listing given listing features?

Even though the charts above gave us a good idea of pricing and availability trends on the platform, it is still not easy task to price a listing. If only we could have a crystal ball that can suggest to us listing prices!

Well, the good news is that using machine leaning is the closest thing we can get to a crystal ball. Therefore, to tackle this question we used the data available to build a machine learning model that can look at the historical pricing data and all property features to suggest a price.

The graph below illustrates the kind of input data that was fed into our predictive model, with the output being the expected price:

![_config.yml]({{ site.baseurl }}/images/ml-model.png)

Without getting into the details, the model performed relatively OK. However, it can be further improved with some possibly extra sources of data. This performance can be illsutrated by looking at the following chart:

![_config.yml]({{ site.baseurl }}/images/actual-vs-predicted.png)

This chart shows the actual prices of a sample of data, and the predictions from the machine learning model. In a perfect scenario, all points should fall on the red line. However, for our case the model is predicting prices with a margin of error of around 25%.

## What are the factors determining the price of a listing?

Now that we have a working model that can make predictions, we can use the model not only to predict prices, but also to identify the most important features in determining the listing price.

The following table lists the top 10 most important features in determining the price ranked from most important:

![_config.yml]({{ site.baseurl }}/images/important-features.png)

Clearly, the most important features to determine a property's price are its size and how many it accomodates. Next, comes the listing type, the level of availability and how much it was reviewed. 

This kind of insight helps us understand for example that property size is more important to some extent that location. Also, it reduces the number of variables that we need to take into consideration when pricing.

## Conclusion

We started with our analysis of pricing from a high level, by looking at trends and seasonality of pricing and availability. This helped us identify the effect of weekends, holidays and so on on those factors.

Then we delved deeper, by training a machine learning model to predict for us an ideal price given different listing features. That was our crystal ball, despite being a bit hazy with the relatively degraded accuracy. Nonetheless, it took us one step closer to identifying the most important factors in pricing.

Finally, we utilized how the model learned to identify those most important factors and that's the most granular level of details that we managed to acquire.

Defnitely, the work can be further improved by utilizing the textual data and analyzing it. Also, including the time factor as a more integral part in the analysis and predictive modeling.
