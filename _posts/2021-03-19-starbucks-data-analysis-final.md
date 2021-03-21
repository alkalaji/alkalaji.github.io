---
layout: post
title: Starbucks Customers and Offers Interaction Analysis
---

Offers are an important tool for engaging your customers, promoting new offerings, or increasing loyalty. However, targeting a customer with the wrong offer could be useless, or in some cases could bring a negative outcome. Therefore, it is very important to understand the interaction between customers and offers.

![_config.yml](https://s11284.pcdn.co/wp-content/uploads/2019/01/starbucks-customer-holding-coffee-cup-700x466.jpg.webp)

## Project Overview

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

__Methodology:__ We will build an offer lifecycle, or a customer journey through an offer, if we look at it from the other way around. For Each offer, we will identify all individual transactions that were targetd with that offer. Then we capture all interaction events (receiving, viewing, completing, or ignoring) an offer. After that we calculate percentages for each event types for each offer.

#### 2. _How did the offers affect or change customer behavior?_ 
This differs by offer type, and the change in behavior could be in terms of spending or frequency of transactions. 

__Methodology:__ First of all, we will identify all customer transactions that are not within an offer period. From that we get transaction amounts. Then of those, we identify the consecutive transactions and calculate the time between transactions. 

The second part applies to BOGO and discount offers, we do the same but for transactions within an offer where we calculate transaction amounts and time between consecutive transactions.

For informational offers, we calculate the time between the first transaction that the customer commits after viewing the offer and the previous transaction. The idea is to see whether the information offer urged the customer to perform a transaction.

Finally, we use a statistical test, specifically z-test, to assess whether there is a statistical significance in the duration between transactions within and without offers on one hand, and the transaction amount on the other hand.

#### 3. _Can we predict if a customer will interact with an offer? and what are the determining factors?_ 
We need to predict given some user info and offer info whether this combination will give a positive outcome. Meaning that offer x suits person y. Furthermore, identify the features are most important to consider when targeting a customer with an offer.

__Methodology:__ We will start by building a dataset that has customer information, some spending info and offer information. The target variable would be whether the offer was viewed and completed by the customer. This is what we want to train our model on. Then after preparing and processig the data, we spilt to train a random forest classifier to predict whether given information about a custmer and a potential offer what is the likelihood of the customer engagng with the offer.

We will tackle those questions one by one throughout the analysis. But before that we need to define the metrics that will be used to assess viability or success of our proposed solutions.

## Metrics

Following are the metrics through which will assess the methodologies and results of the questions in our problem statement:
#### 1. _How did Starbucks customers engage with the different offers?_
For each offer, we will identify all individual transactions that were targetd with that offer. Then we will calculate different metrics.

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

__Metrics:__ Given that we are using z-test we will on two main values:
* p-value: where if p-value < 0.05, then there is a statistically significant difference. Meaning that there is a significant impact by the offer on customer behavior
* z-score: This is what gives us the magnitude of difference in terms of standard deviations difference, and whether it is positive or negative

#### 3. _Can we predict if a customer will interact with an offer? and what are the determining factors?_ 
In comparison to the previous one, this is straight forward.

__Metrics:__ Since this is a classification problem we will be looking at the following metrics:
* Accuracy: to get an overall sense of how the model has preformed. However, this is not a conclusive metric especially with imbalance
* F1 score: since this metric takes precision and recall into consideration. So it is necessary to look at this metric along with accuracy
* Precision and recall: just to get an idea of where might be some bias or other fitting issues with the model

## Data Understanding
**Note: The source of the description below comes from the provided information about the datasets**

The data that will be used is provided in the following three files:
* portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)
* profile.json - demographic data for each customer
* transcript.json - records for transactions, offers received, offers viewed, and offers completed

Here is the schema and explanation of each variable in the files. We also look at the head of each of the three data files, their shape and if there is any missing values

**portfolio.json**
* id (string) - offer id
* offer_type (string) - type of offer ie BOGO, discount, informational
* difficulty (int) - minimum required spend to complete an offer
* reward (int) - reward given for completing an offer
* duration (int) - time for offer to be open, in days
* channels (list of strings)

![_config.yml]({{ site.baseurl }}/images/starbucks/1.png)

**profile.json**
* age (int) - age of the customer 
* became_member_on (int) - date when customer created an app account
* gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)
* id (str) - customer id
* income (float) - customer's income

It was mentioned that missing age values where encded as 118 in the dataset. Hence, we will set those values to nan

![_config.yml]({{ site.baseurl }}/images/starbucks/2.png)

Only profile dataset contains missing values. We can see that around 12.8% of demographics data is missing. We will decide how to handle this as we have a further look at the data

**transcript.json**
* event (str) - record description (ie transaction, offer received, offer viewed, etc.)
* person (str) - customer id
* time (int) - time in hours since start of test. The data begins at time t=0
* value - (dict of strings) - either an offer id or transaction amount depending on the record

![_config.yml]({{ site.baseurl }}/images/starbucks/3.png)

## Data preparation and exploration
In this section we will perform the following data processing steps on each dataset:

### a. "portfolio" dataset
The portfolio data is quite straight forward having only 10 rows corresponding to the 10 offers. We do some basic feature engineering. This makes it easier for any modelling step later on. We do the following steps:
- One-hot-encode channels
- One-hot-encode offers
- Give code names for offers for easier readability

![_config.yml]({{ site.baseurl }}/images/starbucks/4.png)

Then we look at how many times each channel was used and the offer types:

![_config.yml]({{ site.baseurl }}/images/starbucks/5.png)

We can see that email channel is used with all offers, so it loses value in terms of modelling, because it does not carry any discriminatory value! 

This should not be confused with the potential importance of it as a channel, but again not for modelling purposes.

![_config.yml]({{ site.baseurl }}/images/starbucks/6.png)

Clearly we have 4 buy-one-get-one, 4 discount offers, and 2 informational

### b. "profile" dataset
First, for preprocessing we will perform the following steps:
- Convert 'became_member_on' to datetime
- Replace 'became_member_on' with 'year_of_membership'
- Replace missing gender values with 'U' for unknown

#### Looking at the dstribution of each column, and seeing if there are outliers within the data

![_config.yml]({{ site.baseurl }}/images/starbucks/7.png)

We will decide what to do with the missing values denoted as 'U' later on. 
However, looking at M and F values, there is some imbalance, but it is not severe. However we will have to be careful when splitting the data and modelling

![_config.yml]({{ site.baseurl }}/images/starbucks/8.png)

The data seems to be normally distributed with some skewness. One reason is that the application contains ages starting from 18 years old. Then we look at the boxplot to see if there are any outliers:

![_config.yml]({{ site.baseurl }}/images/starbucks/9.png)

We don't see any extreme valus or severe skewness other than slight skewness to the right. 
Also, from the above boxplot, we can see that 50% of our customers fall between ages around 42 and 65

Then we look at Histogram for our newly created feature 'year_of_membership'

![_config.yml]({{ site.baseurl }}/images/starbucks/10.png)

We can see that 50% of customers are in their first year of membership. 40% are in their 2nd or 3rd year

Next, we look at Income histogram ignoring nan values:

![_config.yml]({{ site.baseurl }}/images/starbucks/11.png)

Now we examine for outliers:

![_config.yml]({{ site.baseurl }}/images/starbucks/12.png)

Again here we don't outliers and there is some skewness slightly to the right. 50% of customers have an income between 50k to 80k.

#### Data correlations
Now, we can start looking at the correlation matrix for profile features

![_config.yml]({{ site.baseurl }}/images/starbucks/13.png)

The correlation heatmap suggests some positive correlation between age and income, which is expected and can be seen better in the next set of charts. However, neither age nor income show correlation with year_of_membership feature.

Then we look at the scatterplot matrix to gain further insights

![_config.yml]({{ site.baseurl }}/images/starbucks/14.png)

Given the charts above, I have the following comments:
- The age vs income charts show a positive correlation. However, clearly it shows how synthetic this dataset is. As in real life we won't have those clear borders.
- Male income is right skewed
- For higher incomes, it seems that female customers or more prominent
- There is nothing much noticeable regarding O and null gender records distributions. Therefore, we will drop records with nan value for demographic info

##### As mentioned, we will be removing customer records with missing data. However, better way to do it is to check using a statistical test specifically the distribution of those records with missing values vs with non-missing values and see if there is any statistical significance in the difference between the two

## c. "transcript" dataset
This is where the bulk of the preprocessing work was done as we were deling with the transactions. First we did some initial preprocessing steps by replacing 'amount' column and creating 3 columns: offer_id, amount, reward

#### Looking at the dstribution of each column, and seeing if there are outliers within the data

Starting with transaction amounts

![_config.yml]({{ site.baseurl }}/images/starbucks/15.png)

The shape of this histogram suggests that we have some extreme values. Therefore, we create a boxplot

![_config.yml]({{ site.baseurl }}/images/starbucks/16.png)

We can clearly see that there are extreme values in the data. Having another look without those extreme values:

![_config.yml]({{ site.baseurl }}/images/starbucks/17.png)

Given the a value of 45 to be a reasonable cutoff value for transaction amount, we found that there are 863 extreme values (above 45). Thereforem, given the small number of outliers in comparison to 306534 transaction, we will drop those values. Now we have another look at the histogram:

![_config.yml]({{ site.baseurl }}/images/starbucks/18.png)

This diagram has an interesting shape. I believe it is mostly due to the pricing of items at Starbucks, and the bump in the chart could be attributed to combination of items purchased. To confirm or refute some pricing data would be required.

#### Data correlations
To get a better picture, we look at the correlation between the different features. However, before that we join the transactions with some socio-demographic data:

![_config.yml]({{ site.baseurl }}/images/starbucks/19.png)

From the above correlation matrix we can observe the follwing:
- There's a very strong positive correlation between income and the transaction amount mean value
- However, there's a negative correlation with less magnitude between income and number of transactions
- The year of membership feature shows a positive correlation with the umber of transactions (remomber that this transactions data is for 1 month only)
- Age is positively correlated with the mean transaction amount value. This is expected since we observed earlier that age is positively correlated with income positively

We take things a tep further by looking at the scatterplot matrix:

![_config.yml]({{ site.baseurl }}/images/starbucks/20.png)

The scatterplot matrix above highlights some of the observations we noticed in the correlation matrix above, in addition to highlighting the following:
- Male total transaction amount is extremely right skewed, whereas for female customers it shows less skewness. We observe that a majority of the male customers fall in the lower end of the total amount spent spectrum
- The same applies to avergae transaction amount where male are the lower end of the spectrum and females exceeding them on the higher end
- When it come to number of transactions the distribution seems to be very similar. The higher shaded area for males can be attributed to the data imbalance
- Regarding users which are not labelled as 'M' or 'F', this includes 'O' and nan, most transactions amount and count are on the very lower end of the spectrum

## 4. Data Analysis, modelling, and evaluation
In this section we will analyze the data in order to answer the different business questions that were posed earlier. However, before we get to the questions one by one, we will need to process transcript data to identify how customers interact with the offers, and how offers affect customer behavior.

For that we built a rigorous function to process transactions. The logic in the 'process_transactions' function produces two main data frames, and 5 other data frames containing raw transaction metrics:
1. __offers_trx_details:__ This DataFrame is concerned with offers, how users interact with them, and the transactions that we assume are associated with them, as follows:
    - For all offers received we calculate how long it took to view the offer and how long to complete if applicable
    - For each person for each offer (bogo or discount) we get all the transactions that happened between offer reception and completion. For those specific transactions we calculate avg time between transactions, mean transaction amount, and how many
    - For informational offers we calculate the time from last transaction before the offer was viewed to the first transaction after the offer is viewed
2. __normal_trx_details:__ Those are transactions that took place outside the conditions outlined above. So they do not fall between an offer view and completion
3. __raw_within_offer_trx_amounts:__ individual transaction amounts that took place during an offer
4. __raw_within_offer_trx_durations:__ duration between consecutive transactions during an offer
5. __raw_first_trx_from_last_durations:__ the duration between first commited transaction after receiving an information offer and last transaction
6. __raw_without_offer_trx_amounts:__ individual transaction amounts that took place outside any offer
7. __raw_without_offer_trx_durations:__ duration between consecutive transactions that took place outside offers

The columns description for each of the two main dataframes is as follows:
1. offers_trx_details:
    - __person_id__: Person id
    - __offer_id__: Offer id
    - __viewed_time__: the time difference between viewing and offer and receiving it. If not viewed it would be nan
    - __completed_time__: the time difference between completing an offer and viewing it, in case it was viewed. If not, then from receiving it. If not completed it would be nan
    - __first_trx_time_from_last_trx__: the time from last transaction before the offer was viewed to the first transaction after the offer is viewed
    - __trx_count__: # of transactions between view/reception and completion
    - __avg_trx_amount__: avergae transaction amount for the aforementioned transactions
    - __avg_time_btwn_trx__: average time between transactions for the aforementioned transactions
2. normal_trx_details:
    - __person_id__: Person id
    - __trx_count__: # of transactions that are not between view/reception and completion
    - __avg_trx_amount__: avergae transaction amount for the aforementioned transactions
    - __avg_time_btwn_consec_trx__: average time between only consecutive transactions for the aforementioned transactions





















![_config.yml]({{ site.baseurl }}/images/starbucks/xxxx.png)


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

## Justification
Looking back at the most important decisions we have made throughtout this analysis, we can highlight the following:
- The rigorous function "process_transactions", despite how big the function is, I preferred to keep it all in one function to reduce the overall number of loops. Furthermore, there were so many conditions to keep track of to qualify a transaction to be considered as part of an offer, or just a normal transaction, and the consecutiveness of transactions. The level of detail that this function provides can open up multiple other areas to explore in the data.
- For question two we use choose z-test for the following reasons:
  * We have a very large sample size. Each sample consists of multiple thousand observations
  * The data points are independent from each other
  * Given the large sample size, we can relax the condition or assumption that the data must benormally distributed
- The rationale behind our decisions for question 3 are as follows:
  * I chose Random Forest because it is less prone to over fitting, due to the way it splits the data and builds multiple trees. Also, does not require normalizing or regularizing the data before feeding it into the model. Finally, does not necessarily require data bining, since it can decide on different ranges for each branch or split
  * We use GridSearchCV, first of all to find a better set of parameters for our model. We could have gobe randomized search for faster results, but the calculation was affordable with gridsearch. Also very importantly, we wanted to make use of the cross-validation that it does so that we ensure that we do not overfit our model
  * Regarding evaluation metrics of the model, we did not only rely on accuracy, but we also looked at f1-score which takes into consideration precision and recall, and works better for datasets with imbalanced labels 

## Conclusion and Reflections

In this exercise we analyzed the customers and offers data from Starbucks. We tried to answer the different business questions.

We started our analysis by trying to look at how customers interacted with the different kinds of offer. We measured what we called customer engagement. Then we looked at the interaction with each offer an offer lifecycle and a customer journey. We also identified the best performing offers and the least peforming according to our definition.

Then we looked at the problem from another angle. We tried to see how offers impacted customer behavior. We followed a statistical method to assess the significance of this effect, and we found the indeed offers affect customer spending and the frequency of transactions. We found that information offers seemed to have a negative impact on the duration until next transaction. On the other hand, BOGO and discount offers increase the average transaction amound and the frequency of transactions during an offer.

Finally, we built a predictive model to help us identify whether an offer would suit a certain customer. It had a reasonable accuracy with around 70%. Also, the model gave us an insgiht at what the important features are in determining whether a customer will enage with an offer or not. The two most important indicators where average customer spending and years of membership.

The work can be further improved with some extra sources of data such as item pricing, or through some further engineering of some of the features. Also, we can model the whole data and problems as a time series problem with sequences of transaction. However, this would require us have some further information about the time of transactions to correlate with weekdays, weekends, and time.
