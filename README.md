# FakeNewsDetection

##ABSTRACT##

Owing to the growing popularity of social media, spammers have made these their prime target to mislead others with an intent to make financial or political gains. In this work, we have developed a fake news detection system which allows users to filter and flag potentially deceptive information. The prediction that a particular news article is deceptive is based on the analysis of previously seen news as well as information available online from reliable sources. A scarcity of deceptive news for predictive modeling, is a major stumbling block in this field. Our model using a Random Forest Classifier achieves an accuracy of 94.87% using a combination of features such as n-grams, shallow syntax, tf-idf and a novel online relevance score feature. For the same model we achieve a Balanced Error Rate(BER) score of 0.115. These promising results encourages us to probe further into this issue to eliminate fraud through fake news.

##Data Exploration and Visualization##

**1.Data Collection using Web scraping**

We have extracted data from the following different sources to get a diverse flavor of news data. 
-Twitter Feed (Fake and Real)
-BBC Database
-UCI News Dataset
-Kaggle fake news dataset 

BBC's news dataset was used off the shelf. Kaggle's fake news dataset had a list of links to fake news articles in a CSV files. We used these links to scrap the fake news data using Beautiful Soup. Additionally we subscribed to Twitter REST Api and extracted the tweets from various twitter handles. For example we extracted tweets from '@TheOnion', '@fakingnews', '@ClickHole', '#rumour' etc. for fake news and from channels like '@CNBC', '@washingtonpost', '@HuffingtonPost',etc for genuine content. With all these extracted data we had close to 100,000 data points with 40% of those representative of fake news class. 30% of the collected data was saved for testing our models. This test data was solely extracted from the Twitter feeds mentioned above.

**2.Data Preprocessing**

The data obtained from these different sources were in different formats and organized in different ways. We preprocessed the data to bring uniformity in the data before they can be used to train any model. The data obtained from the twitter feed contained re-tweets, twitter handles and hyper-links. These had to be removed. Also tweets with supporting images were removed from the dataset since at this point of time we are not interested in identifying fake news from images or other media content. We also preprocessed the data obtained from non-twitter sources and trimmed the length,taking only an excerpt of the news. This was done so as to avoid skew in the content length, since tweets are much shorter in comparison to news articles. Also we removed tweets which where very small(less than 5 distinct words) as they do not provide much information to the prediction. Average length of data snippets used for the training was close to 120 characters.

**3.Data Visualization**

After the preprocessing phase, exploratory analysis of the data was done. We initially began by generating a word cloud of the data from fake and real news as shown in the following figures. A quick glance through the images Fig.1 and Fig.2 show that the fake news is mostly related to the 2016 US elections while the true news is not specific to any particular genre. 


![Fake word cloud](https://github.com/gowthu2712/FakeNewsDetection/blob/master/fake_word_cloud.png "Fig.1 Fake word cloud")
Fig.1 Fake word cloud
![True word cloud](https://github.com/gowthu2712/FakeNewsDetection/blob/master/true_word_cloud.png "Fig.2 True word cloud")


As we stated in the previous section, most of the existing work on fake news detection concentrates on the syntactic and semantic features of the content. While in the real world we could have a fake news well disguised under the features of a real news. A trivial predictor would miss out on identifying such fake contents. So we have tried to use a new feature where we measure the current relevance of the given news snippet. We are doing this by finding the Jaccard similarity between the given news and the results from a Bing search. For this purpose, we scrapped data from Bing search query results using Beautiful Soup. We hypothesized that true news articles will have a higher similarity while the fake articles will have low similarity. As a proof of correctness we tested our hypothesis on a small set of 20 fake news snippets and 20 true news snippets and found that the results support our hypothesis as shown in Fig.3(here the blue bubbles are fake news and red bubbles are true news).

![Relevance scores for Fake and Real News](https://github.com/gowthu2712/FakeNewsDetection/blob/master/ors.png "Fig.3 Relevance scores for Fake and Real News")
![Centering Resonance analysis for fake news](https://github.com/gowthu2712/FakeNewsDetection/blob/master/network_false.png "Fig.4 Centering Resonance analysis for fake news")
![Centering Resonance analysis for true news](https://github.com/gowthu2712/FakeNewsDetection/blob/master/network_true.png "Fig.5 Centering Resonance analysis for true news")

Fig.4 and Fig.5 show a networkX model of the fake and real news snippets. As we can see these graphs are densely connected which shows that a bi-gram model could work well in this classification task. These networks are generated using a technique called centering resonance analysis, a type of network based text analysis which represents the content of large data sets by identifying the most important words that link other words. In these two figures we have included only sentences with the word "President" to have close look at the edge distribution of the network. 

We also wanted to find the correlation between the sentiment of the news article and it's type. We used NLTK sentiment analyzer library to compute the sentiment scores for the news articles and got the results as shown in the table below. As we can see, there isn't statistically significant difference between true and fake news in terms of the sentiment of the news.

|News Type | positive | negative | neutral |
|----------|----------|----------|---------|
|True News |  0.0543  |  0.0516  |  0.8792 |
|Fake News |  0.0664  |  0.0571  |  0.8710 |


