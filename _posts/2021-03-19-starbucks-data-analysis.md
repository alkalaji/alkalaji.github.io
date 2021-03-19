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

## How did Starbucks customers engage with the different offers?
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


