# Negation_handling

# Introduction
The layman's notion is that the accuracy of a Sentiment Classification model is mainly dependent on the quality and functionality of the model itself. However, this is not entirely true. The quality of the data and its usability for a model is the main defining characteristic of any prediction driven task. Data Scientists all over the world spend at least 80% of their time in Data Wrangling (as said by Andrew Ng, CEO of Landing AI). Exploratory Data Analysis, Feature Extraction, removing noise, choosing the right model for the data and then making the data ready as input for the model are the most time-taking steps in any process aiming to derive insights from data. Moreover, these take place even before any Machine Learning or Deep Learning algorithms are chosen, which can get extremely complex in nature.
The research done here, shows that after employing Negation Handline and thereby increasing the quality of data the accuracy of Sentiment Classification using simple algorithms like Logistic Regression and Naive Bayes classifier show comparable results to complex Word Embedding models.

For more details kindly refer to my [Medium Article](https://utkarsh-lal.medium.com/increasing-accuracy-of-sentiment-classification-using-negation-handling-b6d2de93445e)

# Methodology
Negation Replacement Methodology
The pseudocode of the function is as follows: -

Alogrithm: Negation Replacement Algorithm
Input: 	sentence, which is a list of words or a	Tokenized sentence
Output: sentence, which is a tokenized Sentence after handling negation

1: FOR i IN range(len(sentence)):
2:      IF sentence[i-1] IN ['not',"n't",'Not','NOT']:
3:          SET antonyms TO [ ]
4:          FOR syn IN wordnet.synsets(sentence[i]):
5:              SET syns TO wordnet.synsets(sentence[i])
6:              SET w1 TO syns[0].name()
7:              FOR l IN syn.lemmas():
8:                  IF l.antonyms():
9:                    antonyms.append(l.antonyms()[0].name())
10:              SET max_dissimilarity TO 0     
#If antonym does not exist then leave it alone.
11:             FOR ant IN antonyms:
12:              SET syns TO wordnet.synsets(ant)
13:              SET w2 TO syns[0].name()
14:              SET syns TO wordnet.synsets(sentence[i])
15:              SET w1 TO syns[0].name()
16:              SET word1 TO wordnet.synset(w1)
17:              SET word2 TO wordnet.synset(w2)
18:              IF isinstance(word1.wup_similarity(word2), 
float) or isinstance(word1.wup_similarity(word2), int):
19:                   SET temp TO 1 - word1.wup_similarity(word2) 
20:                  IF temp>max_dissimilarity:
21:                      SET max_dissimilarity TO temp
22:                      SET antonym_max TO ant
23:                      SET sentence[i] TO antonym_max
24:                      SET sentence[i-1] TO ''
25:    WHILE '' IN sentence:
26:      sentence.remove('')
27:    RETURN sentence


In the function, whenever a negation word (like ‘not’, “n’t”, ‘non-‘, ‘un-‘, etc) is encountered, a set of cognitive synonyms called synsets are generated for the word next to the negation. These synsets are interlinked by conceptual semantic and lexical relations to each other in a lexical database called WordNet. WordNet is a part of the NLTK python library. After the synsets are created, the method checks if an antonym of that word exists in the WordNet.
If an antonym doesn’t exist, it implies that the word is either a verb or an entity that has no opposite word available in the WordNet. So, it is left as it is, and the context of the sentence is preserved. But if an antonym exists in the lexical database, then a list of antonyms is created. Then the dissimilarity coefficient of each of these antonyms are found using the function, wup_similarity. The dissimilarity coefficient of two words would be: - 
dissimilarity = (1 - word1.wup_similarity(word2))
Out of all the antonyms, one with the highest dissimilarity is considered and the first word next to the Negation is replaced with that antonym. Then the negation word is removed, and we get a sentence with inverted polarity as a result.  Replacing with the antonym that has the maximum dissimilarity ensures that the maximum inversion of polarity takes place. This prevents words like “beautiful” being replaced by something like “bad”, which is an indirect antonym. Consider another example as shown below: 
 
![image](https://user-images.githubusercontent.com/29978378/154960752-426a78d8-e938-4a1d-8148-68505afd9528.png)

Synset
It is a special sort of interface residing in the NLTK library which helps in looking up words in the WordNet lexical database. Synset instances are groupings of synonymous words that express the same concept. Using Synsets in this paper for negation handling gives a much simpler way to find relationships between different words than training complex models or using heavy duty word embeddings, which take much higher processing power and other resources. 

#Results
| Tables                                | Naïve Bayes Classifier | Logistic Regression |
| ------------------------------------- |:----------------------:| -------------------:|
| BoW with Uni+Bigrams and Negation     | 66.90%                 | 70.88%              |
| BoW with Uni+Bigrams without Negation | 66.32%                 | 70.22%              |
| TFIDF (Uni+Bigrams) with Negation     | 64.35%                 | 70.25%              |
| TFIDF (Uni+Bigrams) without Negation  | 64.35%                 | 70.25%              |

![image](https://user-images.githubusercontent.com/29978378/154961308-49b6df53-baa6-4ad8-bf5b-ea1737255368.png)

![image](https://user-images.githubusercontent.com/29978378/154961335-6f7cfa8c-2448-4429-be88-ab7622a6d3e6.png)


# Conclusion
The quality of data is one of the most important factors in Sentiment Classification. Good Data can give good results when combined with the appropriate model. For industrial applications of Sentiment Analysis, where a quick turnaround time is required with minimum cost and maximum resource utilization, using heavy and complex algorithms often incur a lot of overhead and resources. Instead of spending time and resources on complex ML and DL algorithms which lie figuratively in the last leg of a Data Pipeline, we should focus on the first leg of the Data Pipeline which is preparing the data and increasing the quality of data. The function implemented in this paper does precisely the same and increases the data quality to an extent that the results are almost comparable to a complex algorithm but uses less resources and simpler Machine Learning models. 
