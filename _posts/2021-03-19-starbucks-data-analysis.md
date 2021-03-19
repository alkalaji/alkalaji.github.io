---
layout: post
title: Starbucks Customers and Offers Interaction Analysis
---

Offers are an important tool for engaging your customers, promoting new offerings, or increasing loyalty. However, targeting a customer with the wrong offer could be useless, or in some cases could bring a negative outcome. Therefore, it is very important to understand the interaction between customers and offers.

![_config.yml](https://s11284.pcdn.co/wp-content/uploads/2019/01/starbucks-customer-holding-coffee-cup-700x466.jpg.webp)

Starbucks is the world's largest coffehouse chain in the world. With thousands of stores, and millions of customers, it is important to engage with your customers and understand them. Therefore, Starbucks has an application that makes it easier to know customers better, make it easier for them to order, and to send offers.

However, with the huge number of customers, and multitude of offer options, it is a challenge to understand how customer interact with the offers, and how the offers affct customers behavior. Therefore, using some Starbucks provided data that mimics real life offers and customer behavior.

The data contains scio-demographic information about customers, the transactions of customers, and the different types of offers which are:
- Informational: An advertisement informing customers about a product
- BOGO: Buy one get one,  where auser needs to spend a certain amount to get a reward equal to that threshold amount
- Discount: a user gains a reward equal to a fraction of the amount spent

Through this analysis, we will try to answer the following business questions:
1. _How did Starbucks customers engage with the different offers?_
2. _How did the offers affect or change customer behavior?_
3. _Can we predict if a customer will interact with an offer? and what are the determining factors?_

We will tackle those questions one by one. So let's get started!

## Q1. How did Starbucks customers engage with the different offers?
Each offer has a lifecycle, and goes through different stages depending on the offer type, from being received to viewed or ignore, to completed. Also, we need to measure the effectivess of an offer. This can be measured in many ways, whether financial, or impact on future purchases, or just the engagement of customers.

For the purpose of this question, we will focus on customer engagement, and how customers interacted with each offer in terms of viewing and completing.

Starting with the viewing percentage for each offer:
![_config.yml]({{ site.baseurl }}/images/starbucks/viewed.png)

We can notice that 6 out of 10 offers were viewed by around 80% or more of customers which seems very good. However, the offers (bogo_3, discount_1, and discount_4) had less than 50% viewing, which suggests that it did not attract customers as the other ones did.

Then we look at the performance of offers in terms of being viewed and completed, and being completed WITHOUT having been viewed:
![_config.yml]({{ site.baseurl }}/images/starbucks/viewed_completed.png)
![_config.yml]({{ site.baseurl }}/images/starbucks/completed_not_viewed.png)

We notice that around 30% of those who completed bogo_3, discount_1, and discount_4 did it without even viewing the offer. This means that either way customers would have continued with their purchaing habits without the business spending money on those offers. However, this cannot be concluded unless we know the motivation of this offer. It might not be purely financial, there might be loyalty aspects, or some utility function that is used to measure success of an offer

Furthermore, surprisingly, those three offers (bogo_3, discount_1, and discount_4) that were viewed the least (less than 50%), are the ones that were completed without being viewed the most. This is unclear and could be due to the way the data was synthesized or some other factors that require clarification from business, because the difficulty of those offers is above average

After all, the offer that seems to have done the best in terms of engagement is "discount_3". Customers seem to have engaged with it in terms of views, completion, and the relatively lower number of those who viewed it and ignored it. On the other hand, "discount_1" seems to have done the poorest in terms of customer engagement, and being completed without customers even noticing it. 

The following chart concisely summarizes the lifecycle of those 2 offers:
![_config.yml]({{ site.baseurl }}/images/starbucks/journey.png)

This kind of diagrams gives a very good view of how each offer is performing. Of course we can add other measures such as financial, or loyalty, if we have data to support.

## Q2. How did the offers affect or change customer behavior?
In the first question we looked at how the offers performed in terms of customer engagement. Now, we want to measure if those offers changed the customer behavior. Of course the condition for transactions within an offer is that the offer has been viewed, and completed in the case of bogo and discount. 

We will measure this as follows:
1. For informational offers, we will check if the duration that users take to make the first purchase after the informational offer is reduced in comparison to the usual duration that customers take between consecutive purchases that are not under any offer
2. For bogo and discount offers, we will check if the duration between purchase instances for transactions within an offer is different from the duration between normal consecutive transactions that are not within an offer period
3. Finally, we will check if the there is a statistically significant change in transaction amounts during an offer (bogo or discount) from outside an offer

Those tests will be conducted using a statistical method (z-test), and then we measure the statistical significance of the result.

Let's start with the tests one by one:
__1. Starting with our test regarding the effect of informational offers:__
![_config.yml]({{ site.baseurl }}/images/starbucks/info_duration.png)

Only by looking at the chart above and the mean values we can see how different the two samples are. Of course, the z-test confirms that informational offer has an effect. However, it is suprisingly counter-intuitive. Because this suggests that customers take longer time to make a purchase after receiving an informational offer in comparison to the time they usually take between transactions.

Nonetheless, this could be due to the time informational offers are beng set. E.g. late at night, or some time in the weekend. This requires further investigation with business, and complimentary data to shed more light.

__2. Testing the effect of BOGO and discount offers on transaction DURATIONS:__
![_config.yml]({{ site.baseurl }}/images/starbucks/duration.png)

Clearly, from the mean values we see above and the ztest, during an offer customers do purchase more frequently. I.e. the duration between purchase transactions is reduced!

Of course if needed we can dig deeper to see which specific offer did best

__3. Testing the effect of BOGO and discount offers on transaction AMOUNTS:__
![_config.yml]({{ site.baseurl }}/images/starbucks/amounts.png)

Again, this clearly shows that during an offer customers spend more per transaction, and the amount difference is statistically significant

## Q3. Can we predict if a customer will interact with an offer? and what are the determining factors?
