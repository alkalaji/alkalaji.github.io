---
layout: post
title: Starbucks Customers and Offers Interaction Analysis
---

Offers are an important tool for engaging your customers, promoting new offerings, or increasing loyalty. However, targeting a customer with the wrong offer could be useless, or in some cases could bring a negative outcome. Therefore, it is very important to understand the interaction between customers and offers.

![_config.yml](https://s11284.pcdn.co/wp-content/uploads/2019/01/starbucks-customer-holding-coffee-cup-700x466.jpg.webp)

## Project Introduction

Starbucks is the world's largest coffehouse chain in the world. With thousands of stores, and millions of customers, it is important to engage with your customers and understand them. Therefore, Starbucks has an application that makes it easier to know customers better, make it easier for them to order, and to send offers.

However, with the huge number of customers, and multitude of offer options, it is a challenge to understand how customer interact with the offers, and how the offers affct customers behavior. 

Therefore, Starbucks provided a simulated dataset that mimics real life offers and customer behavior.

The data contains scio-demographic information about customers, the transactions of customers, and the different types of offers which are:
- Informational: An advertisement informing customers about a product
- BOGO: Buy one get one,  where auser needs to spend a certain amount to get a reward equal to that threshold amount
- Discount: a user gains a reward equal to a fraction of the amount spent

In this writeup, we document the challenges and motivation behind this problem. Then, we define a a problem statement that will guide our efforts throughout the analysis. After that, we outline the methodology that we will use to solve the defined problem, and what metrics we will use assess the outcomes. Overall, we follow the CRISP-DM approach in our analysis.

## Challenge and Motivation

Given the millions of customers and the limitless options of offers, it is a challenge to know which offer to send to what customer. Furthermore, sending the wrong offer to a customer might impact customer's loyalty, or change the perception of a customer into thinking that those offers are more like spam. 

Even from a financial perspective, usually an offer means cost for Starbucks, whether directly or indirectly as outlined above. This uncertainty is what makes this problem challenging, and gives us a great motive to tackle in order to know what to send to whom and understand the dynamics of offer and customers and how they interact and affect each other.

## Problem Statement

With the outlined challenges that we have, and using the simulated data provided by Starbucks, we need to answer the following question:
#### 1. _How did Starbucks customers engage with the different offers?_ 
Or put another way, we want to know how our offers performed, the level of engagement they had, and whether they had some utility.

#### 2. _How did the offers affect or change customer behavior?_ 
This differs by offer type, and the change in behavior could be in terms of spending or frequency of transactions. 

#### 3. _Can we predict if a customer will interact with an offer? and what are the determining factors?_ 
We need to predict given some user info and offer info whether this combination will give a positive outcome. Meaning that offer x suits person y. Furthermore, identify the features are most important to consider when targeting a customer with an offer

We will tackle those questions one by one throughout the analysis. But before that we need to define the metrics that will be used to assess viability or success of our proposed solutions.

## Methodology and Metrics

Following is the methodology through which will follow to answer each of the questions in our problem statement:
#### 1. _How did Starbucks customers engage with the different offers?_
For each offer, we will identify all individual transactions that were targetd with that offer. Then we will calculate different metrics.

__Methodology:__ We will build an offer lifecycle, or a customer journey through an offer, if we look at it from the other way around. For Each offer, we will identify all individual transactions that were targetd with that offer. Then we capture all interaction events (receiving, viewing, completing, or ignoring) an offer. After that we calculate percentages for each event types for each offer.

__Metrics:__ The percentages that will be calculated will be our metrics to assess how the offers performed. Namely, we will have the following metrics to asess, for each offer:
* Viewing percentage: What percentage has the offer been viewed. Higher value means that at least the offer reached the customer
__<div align="center"> viewing% = views_count/sent_count </div>__

* Completed percentage: The percentage an offer has been completed. This will be further broken down in the following two metrics
__<div align="center"> completed% = completed_count/sent_count </div>__

* Viewed and completed percentage: The percentage an offer has been completed after being viewed. This is important because it means that we had a fruitful engagement to some extent.
__<div align="center"> viewed_and_completed% = viewed_and_completed_count/sent_count </div>__

* Completed but NOT viewed percentage: The percentage an offer has been completed without being viewed. High percentage indicates that the offer was too easy to complete, and that customers completed it unintentionally even. So it might have limited impact.
__<div align="center"> completed_not_viewed% = (completed_count - viewed_and_completed_count)/sent_count </div>__

* Viewed and ignored percentage: The percentage an offer has been viewed, but the customer did not complete. This is also important because it means that despite seeing the offer, to some extent the customer either had no interest in the offer, or it was too difficult to complete.
__<div align="center"> viewed_and_ignored% = (views_count-viewed_and_completed_count)/sent_count </div>__

#### 2. _How did the offers affect or change customer behavior?_ 
Here we go through all transactions to identify those within an offer and those outside. Then we statistically test if there is some significant differnece between the two.

__Methodology:__ First of all, we will identify all customer transactions that are not within an offer period. From that we get transaction amounts. Then of those, we identify the consecutive transactions and calculate the time between transactions. 

The second part applies to BOGO and discount offers, we do the same but for transactions within an offer where we calculate transaction amounts and time between consecutive transactions.

For informational offers, we calculate the time between the first transaction that the customer commits after viewing the offer and the previous transaction. The idea is to see whether the information offer urged the customer to perform a transaction.

Finally, we use a statistical test, specifically z-test, to assess whether there is a statistical significance in the duration between transactions within and without offers on one hand, and the transaction amount on the other hand.

__Metrics:__ Given that we are using z-test we will on two main values:
* p-value: where if p-value < 0.05, then there is a statistically significant difference. Meaning that there is a significant impact by the offer on customer behavior
* z-score: This is what gives us the magnitude of difference in terms of standard deviations difference, and whether it is positive or negative

#### 3. _Can we predict if a customer will interact with an offer? and what are the determining factors?_ 
In comparison to the previous one, this is straight forward.

__Methodology:__ We will start by building a dataset that has customer information, some spending info and offer information. The target variable would be whether the offer was viewed and completed by the customer. This is what we want to train our model on. Then after preparing and processig the data, we spilt to train a random forest classifier to predict whether given information about a custmer and a potential offer what is the likelihood of the customer engagng with the offer.

__Metrics:__ Since this is a classification problem we will be looking at the following metrics:
* Accuracy: to get an overall sense of how the model has preformed. However, this is not a conclusive metric especially with imbalance
* F1 score: since this metric takes precision and recall into consideration. So it is necessary to look at this metric along with accuracy
* Precision and recall: just to get an idea of where might be some bias or other fitting issues with the model



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
For this purpose, we created a dataset containing:
- Socio-demographic information about customers
- Offer details
- Customer usual spending on transactions

Then we added a flag of whether each customer viewed and completed the offer. This will be our target for prediction.

This dataset was used to train and evaluate a machine learning model that will try to predict given customer and offer info, whether the customer will view and complete this offer or not

![_config.yml]({{ site.baseurl }}/images/starbucks/ml-model.png)

The built model performed relatively well with an accuracy of around 73%. Of course, we did not rely only on accuracyas it can be prone to biases and imbalance. Therefore, we evaluated the model with f1-score, and it reported a value of 0.68, which is very reasonable given the accuracy.

This model now enables us to infer whether a customer will engage with an offer or not before sending out the offer.

Finally, let's identify what are the most important features determined by the model that affect whethe a customer will engage with an offer. One way to do that is by looking at features importance:

![_config.yml]({{ site.baseurl }}/images/starbucks/features_imp.png)

We can clearly see that individual attributes are the most important features, followed by offer properties. Those features are:
- avg_trx_amount: The average spending amount per transaction of a customer
- year_of_membership: How long the customer has been a member
- social_ch: whether the offer was sent of a social media channel or not
- income: Customer's income
- age: Customer's age
- reward: The reward amount of the offer
- difficulty: Offer's difficulty
- duration: How long the offer will run for
- mobile_ch: Whether the offer was sent over the mobile channel
- If the offer is of type buy-one-get-one

Now let's look at how those features are correlated with the outcome and with each other. However, we will limit it only to 5 features for ease of visualization:

![_config.yml]({{ site.baseurl }}/images/starbucks/scatterplot.png)

Focusing on the diagonal charts, we can observe the following:
- Average transaction amount is positively correlated with likelihood of participating
- Years of membership that are between 2 and 3 are more positively correlted with a positive outcome. Less or more years are the other way around. Especially newer members.
- Social channel shows a positive correlation
- The higher the income the more likely someone seems to participate in an offer

## Conclusion

In this exercise we analyzed the customers and offers data from Starbucks. We tried to answer the different business questions.

We started our analysis by trying to look at how customers interacted with the different kinds of offer. We measured what we called customer engagement. Then we looked at the interaction with each offer an offer lifecycle and a customer journey.

Then we looked at the problem from another angle. We tried to see how offers impacted customer behavior. We followed a statistical method to assess the significance of this effect, and we found the indeed offers affect customer spending and the frequency of transactions.

Finally, we built a predictive model to help us identify whether an offer would suit a certain customer. Also, the model gave us an insgiht at what the important features are in determining whether a customer will enage with an offer or not. 

The work can be further improved with some extra sources of data such as item pricing, or through some further engineering of some of the features.
