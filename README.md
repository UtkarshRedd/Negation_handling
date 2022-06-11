# Negation_handling

# Introduction
The layman's notion is that the accuracy of a Sentiment Classification model is mainly dependent on the quality and functionality of the model itself. However, this is not entirely true. The quality of the data and its usability for a model is the main defining characteristic of any prediction driven task. Data Scientists all over the world spend at least 80% of their time in Data Wrangling (as said by Andrew Ng, CEO of Landing AI). Exploratory Data Analysis, Feature Extraction, removing noise, choosing the right model for the data and then making the data ready as input for the model are the most time-taking steps in any process aiming to derive insights from data. Moreover, these take place even before any Machine Learning or Deep Learning algorithms are chosen, which can get extremely complex in nature.
The research done here, shows that after employing Negation Handline and thereby increasing the quality of data the accuracy of Sentiment Classification using simple algorithms like Logistic Regression and Naive Bayes classifier show comparable results to complex Word Embedding models.

For more details kindly refer to my [Medium Article](https://utkarsh-lal.medium.com/increasing-accuracy-of-sentiment-classification-using-negation-handling-b6d2de93445e)

# Methodology
In the function, whenever a negation word (like ‘not’, “n’t”, ‘non-‘, ‘un-‘, etc) is encountered, a set of cognitive synonyms called synsets are generated for the word next to the negation. These synsets are interlinked by conceptual semantic and lexical relations to each other in a lexical database called WordNet. WordNet is a part of the NLTK python library. After the synsets are created, the method checks if an antonym of that word exists in the WordNet.
If an antonym doesn’t exist, it implies that the word is either a verb or an entity that has no opposite word available in the WordNet. So, it is left as it is, and the context of the sentence is preserved. But if an antonym exists in the lexical database, then a list of antonyms is created. Then the dissimilarity coefficient of each of these antonyms are found using the function, wup_similarity. The dissimilarity coefficient of two words would be: - 
dissimilarity = (1 - word1.wup_similarity(word2))
Out of all the antonyms, one with the highest dissimilarity is considered and the first word next to the Negation is replaced with that antonym. Then the negation word is removed, and we get a sentence with inverted polarity as a result.  Replacing with the antonym that has the maximum dissimilarity ensures that the maximum inversion of polarity takes place. This prevents words like “beautiful” being replaced by something like “bad”, which is an indirect antonym. Consider another example as shown below: 
 
![image](https://user-images.githubusercontent.com/29978378/154960752-426a78d8-e938-4a1d-8148-68505afd9528.png)

Synset
It is a special sort of interface residing in the NLTK library which helps in looking up words in the WordNet lexical database. Synset instances are groupings of synonymous words that express the same concept. Using Synsets in this paper for negation handling gives a much simpler way to find relationships between different words than training complex models or using heavy duty word embeddings, which take much higher processing power and other resources. 

#Results
| Tables                  | Just Preprocessing     | With Negation       |
| -----------------------:|-----------------------:| -------------------:|
| BoW with Bigrams        | 76.70%                 | 81.40%              |
| BoW with Uni+Bigrams    | 85.15%                 | 87.30%              |
| BoW with Unigrams       | 84.25%                 | 85.20%              |
| TFIDF with Bigrams      | 77.15%                 | 82.35%              |
| TFIDF with uni+bigrams  | 86.35%                 | 87.50%              |
| TFIDF with Unigrams     | 85.35%                 | 87.30%              |
| Word Embedding          |                        | 90.20%              |

![image](https://user-images.githubusercontent.com/29978378/154961308-49b6df53-baa6-4ad8-bf5b-ea1737255368.png)

![image](https://user-images.githubusercontent.com/29978378/154961335-6f7cfa8c-2448-4429-be88-ab7622a6d3e6.png)


# Conclusion
The quality of data is one of the most important factors in Sentiment Classification. Good Data can give good results when combined with the appropriate model. For industrial applications of Sentiment Analysis, where a quick turnaround time is required with minimum cost and maximum resource utilization, using heavy and complex algorithms often incur a lot of overhead and resources. Instead of spending time and resources on complex ML and DL algorithms which lie figuratively in the last leg of a Data Pipeline, we should focus on the first leg of the Data Pipeline which is preparing the data and increasing the quality of data. The function implemented in this paper does precisely the same and increases the data quality to an extent that the results are almost comparable to a complex algorithm but uses less resources and simpler Machine Learning models. 
