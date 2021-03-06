# Using City Subreddits as a Social Sentiment Indicator


## README Contents

- [Introduction](#introduction) 
- [Executive Summary](#methodology)
- [Analysis and Findings](#analysis)
    - [Comparison of Frequently Used Words for Each Subreddit](#freq)
    - [Comparison of Regressions for Models](#regressions)
    - [Confusion Matrix & Classification Metrics](#cm)
- [Conclusions](#conclusion)
- [Recommendations, & Further Application](#further)
- [Sources](#sources)


# Introduction  <a id='introduction'></a>

NYC and Los Angeles are the epicenters of the East and West Coast of the United States, two of the largest and richest cities in the world. They have many similarities both being major cities that are hubs for cultural diversity, start ups, finance, and an endless amount of entertainment. On the flip side, both cities face similar problems such as rising transportation costs, rising rents, homelessness, and crime. 

For city planners and government officials, it is important to try and guage the values of a city. What do people in this city do in their leisure time? What do they hate about the city? What do they worry about? What needs to be improved? 

In this project we aim to look at the most talked about topics on two subreddits, r/nyc and r/losangeles as well as see how unique each of the topics are by seeing if we can predict a city by words found in a title of a post using Natural Language Processing and two models, Logistic Regression and Multinomial Naive Bayes.

If our model can accurately predict the correct city this might mean that the cities have different discussions whereas a model that struggles to accurately predict means the cities have similar discussions. 

A possible application for the scores from this model is to become an indicator that can be utilized by politicians on a campaign trail as they can make decisions on what topics to address in which cities. Our plotted model score over time will be able to help solve and answer the following questions. Are these two cities discussing similar things right now or are there discussions very different at the moment? Was there an observable difference in discussions at this point in time? Our model score can serve as a starting point for further investigation into cities differing or similar issues or interests.



# Executive Summary <a id='methodology'></a>
We started with gathering data from each subreddit, r/nyc and r/losangeles by using the Push Shift API created by Jason Michael Baumgartner which provided us with the means to gather submission and comment data for each subreddit. Our initial data was composed from posts from the last 150 days. The data included the title of the post, the user, number of comments, data created, self text, and a score.

To clean the data we removed all duplicate titles and then cleaned the actual text of the title by removing characters and words that provided little or redundant information. Before modelling we examined the most frequently appearing terms for each subreddit where we found that many of the terms were the same, including distribution of that term. 

To find the best model we used three approaches. Our logistic regression models used both Count Vectorizer and Term Frequency Inverse Document Frequency Vectorizer with GridSearch. Our Multinomial Naive Bayes model used Count Vectorizer. We train test split our feature (subreddit title) and target y (subreddit) using the default value of 0.75 for training. 

Our metrics for determining the best model was to look at both train and test scores for each of our models and choose the model that had the best combination of accuracy and low bias/variance. From that model we then looked at classification metrics: Accuracy,  Misclassification Rate, Sensitivity, Specificity, and Precision to see which part of our model needed the most work.


# Analysis and Findings <a id='analysis'></a>

## Comparison of Most Frequently Used Words in Each Subreddit<a id='freq'></a>
<img src="Project_Files/Images/freq_nyc.png"  width="800" height="450">

<img src="Project_Files/Images/freq_la.png"  width="800" height="450">

From these graphs we can see that both cities' subreddits have very similar word usage in their title. As with NYC, Los Angeles reddit users talk a lot about their concerns with transportation as well as sexual assault. 'Billion' comes up quite frequently which refers to topics regarding budget spending. They also talk about Trump quite frequently but do not talk about other politicians as much as New York. Suprisingly Buffalo was also found here frequently which upon further research was found to represent buffalo wings unlike NYC's subreddit which is referring to Buffalo, NY. Also interesting to note that the NYC subreddit does not talk about NYPD as much as one would think. Some unique terms that were discussed in the LA subreddit were earthquakes and homelessness. 


## Comparison of Regressions for Models <a id='regressions'></a>


**Note**: Reported values for the model change over time as our model is based on the most recent posts in each subreddit. The following scores and analysis were for 1/29/2020.

**Baseline Model**: 0.54

**Following logistic regression scores are with grid search**:

|   Model 	|   Logistic Regression Count Vectorizer	|   Logistic Regression TFIDF Vectorizer	|   Multinomial Naive Bayes	|   	|
|---	|---	|---	|---	|---	|
|   Test	|  0.641 	|  0.645 	|   0.598	|   	|
|   Train	|   0.622	|   0.624	|   0.923|   	|

Of all the models our **Count Vectorizer Logistic Regression** model using Grid Search provided the best results. It produced a training score of 0.641 and a testing score 0.622 Although our TFIDF Vectorizer provided similar results, the variance was less with our Count Vectorizer model. Our Multinomial Naive Bayes Model was severely underfit as our training score was roughly 0.62 and our test score was 0.92. 

Our Count Vectorizer Logistic Regression model shows us that NYC and LA are very similar cities because it had a difficult time differentiating between the two cities. It only performed slightly better than our baseline of 54%. The reason for this is most likely because many of the  most frequently used words for each subreddit were the same even in their distribution.



## Confusion Matrix Metrics<a id='cm'></a>




<img src="Project_Files/Images/cm.png"  width="450" height="450">

|  Metric	| Accuracy 	| Misclassification Rate  	| Sensitivity  	| Specificity  	| Precision  	|
|---	|---	|---	|---	|---	|---	|
| Score 	|  62% 	| 38%  	| 87%  	| 32%   	| 60% 	|


The metric that stands out the most is specificity. Our model did very poor in identifying when a topic was from the NYC subreddit. The reason for this is likely as stated before that many of NYC's most frequently used words are also LA's most frequently used words. Another reason why our model struggles in this area might be because of our uneven distribution of data. We have roughly 15% less datapoints for NYC than we do for LA which means our model is better trained to predict LA as the subreddit.



# Conclusions <a id='conclusion'></a>

To conclude LA and NYC are very similar in terms of discussions on each respective subreddit. Our low model score which is close to our baseline shows that these two cities talk about very similar topics. They have very similar concerns involving sexual assault, transportation, budget spending, Trump, and even terms that might seem unusual such as Buffalo. This is however only a snapshot of these two subreddits taken over the last 150 days. The score of this model can change over time meaning this model could become more accurate. If we were to improve the model we might pull more data using our API, perhaps 2000+ posts from each would improve the model while also making sure we have an even amount of datapoints.


# Recommendations and Further Applications <a id='further'></a>

The application of our project and our model has several use cases as described in the problem statement in regards to a social sentiment indicator that can be used for politicians and goverment.

**Use 1** (Non model related): As a starting point for policy ideas. We know that policy statistics don't always reflect the sentiment of a public. We can scrape social media data for all major US cities and do frequency counts on select terms. For example we could do counts on terms relating to assault and see which cities have a low frequency of these words in their Reddit or other social medias, meaning that issue might not be as a concern for them. From there, policymakers have a starting point. What are those city's policies? Why do those policies work for that city? Additionally one could plot the frequency of a term over time. For example if one major city underwent a minimum wage change and another did not, we could see how often people started to discuss economic terms such as unemployment, wages, rents, spending over time to get feedback faster rather than wait on lagging economic indicators.

**Use 2**: Uses our model score also as a starting point to guage sentiment between cities. If we can plot the score for our model between two neighboring or similar cities as a function of time we can see at which points in time the cities are socially similar (low model score) and socially different (high model score). If a politician wanted to hone in on a specific issue, he/she could see when the score was really high between two similar cities. From there he/she can then look at the topic that was most talked about for those cities and really see what was specific to the city.

**Limitations of Our Model**:
- Only includes opinions from the subset of people who use social media (generally skews younger in age).
- Opinion bias as most people use social media to voice concerns/complainst and not the positives of a policy.
- Smaller cities have a smaller online footprint and might not have as many datapoints to work with.



# Sources<a id='sources'></a>

* API Used: https://github.com/pushshift/api


```python

```
