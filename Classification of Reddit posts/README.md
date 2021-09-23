# Project 3 Web APIs and NLP 

## Contents

1. [Introduction](#Introduction)
2. [Data Cleaning and EDA](#Data-Cleaning-and-EDA)
3. [Baseline Model](#Baseline-model)
4. [Comparison of performance of ML classification models](#Comparison-of-performance-of-ML-classification-models)
5. [Further analysis of the best gradient boosting model ](#Further-analysis-of-the-best-gradient-boosting-model)
6. [Conclusions and TODO](#Conclusions-and-TODO)

## Introduction

**About Reddit**

[Reddit](https://www.reddit.com/) is an American social news aggregation, web content rating, and discussion website. Registered members submit content to the site such as links, text posts, images, and videos, which are then voted up or down by other members. Posts are organized by subject into user-created boards called "communities" or "subreddits", which cover a variety of topics [(source)](https://en.wikipedia.org/wiki/Reddit). 

**Goals**

This analysis aims to scrape data from two subreddits and build a machine learning model to classify them. 

**The subreddits**

The subreddits being used in this analysis and their descriptions from [Reddit](https://www.reddit.com/) are given below:

- [r/languagelearning](https://www.reddit.com/r/languagelearning/)

    This is a subreddit for anybody interested in the pursuit of languages. Whether you are just starting, a polyglot or a language nerd, this is the place for you!
        
        
- [r/linguistics](https://www.reddit.com/r/linguistics/)

    \*\*lin⋅guis⋅tics\*\*: the scientific study of human \*language\* \* what form does it take? \* how is meaning constructed? \* how is it structured? \* how is it produced?

**Problem Statement**

[Section 3](#Baseline-model) elaborates on a basic model with an accuracy of 74%. The aim is to build more sophisticated models and improve on this baseline accuracy

## Data Cleaning and EDA

Data cleaning and EDA details are available in this [notebook](https://git.generalassemb.ly/sumakarnam/dsir-82/blob/master/projects/project3/Code/EDA.ipynb). The scraped reddit data was cleaned by discarding any posts that were removed or had no content except for the title. 

The EDA involved an analysis of the numerical features of the scraped data (character and word counts of the title and text of the post and the number of comments). As the distributions of these numerical features were similar for both the subreddits, the current analysis uses only the text data to classify the posts. 

The following cell shows the similarity in the distributions of word counts of posts in both the subreddits

![distribution%20_post_%20word_count.png](./Data/Figures/distribution%20_post_%20word_count.png)

## Baseline model

Two simple baseline models were built by considering the most common words that occur in one of the subreddits but not the other.

For each subreddit, a list of frequent words that occur in it but not frequently in the other is constructed.

- Model 1: If a post contains at least one word from this list for r/languagelearning, it is classified to be from r/languagelearning, else r/linguistics.
- Model 2: If a post contains at least one word from this list for r/linguistics, it is classified to be from r/linguistics, r/languagelearning.

These models were iterated for various lengths of the frequent words list.

Best Accuracies of these models were observed to be as follows:
- Model 1: 0.74
- Model 2: 0.61

## Comparison of performance of ML classification models

The goal of this section is to improve upon the accuracy of the baseline models using various classification algorithms. The text data was featurized using CountVectorizer and the following classification models were used:

- Random Forest 
- Gradient Boosting
- Voting Classifier

The hyperparameters for the CountVectorizer and the estimators were tuned using a GridSearch. 

For each of the above models, the subreddit posts were classified using three different feature sets.
- model 1 : using the title of the post
- model 2 : using the content (selftext) of the post
- model 3 : using both the title and content of the post

The models using the text from both the title and content of the post (model 3) outperformed the other two models.
The following table summarizes the accuracies achieved with each algorithm

|                   | Overall Accuracy |           |
|-------------------|:----------------:|:---------:|
|                   |   Training data  | Test Data |
|   Random Forest   |       0.86       |    0.82   |
| Gradient Boosting |       0.88       |    0.83   |
| Voting Classifier |       0.89       |    0.83   |

## Further analysis of the best gradient boosting model 

This section is a deeper dive into the gradient boosting model and its performance for various values of the CountVectorization parameters. 

Key take-away:

- 1,2,3 and 4 grams are important in the model. The higher n_grams don't affect model performance considerably

![cross%20val%20vs%20n%20grams.png](./Data/Figures/cross%20val%20vs%20n%20grams.png)

## Conclusions and TODO

**Conclusions**

- The best performing model (a voting classifier) in the analysis led to a lift of about 9% in accuracy (from 74% to 83%) from the baseline model
- All the classification algorithms had a better accuracy in r/linguistics compared to r/languagelearning
- Using the text from both the title and content of the post resulted in better performance of all the models

**TODO**

- Include numerical features - length of post, character count, number of comments etc in the analysis
- Preprocess the text - stemming, lemmatizing, augmenting the stopwords
- Try other kinds of vectorization of the text (TfidfVectorizer)
- Analyze larger samples of data
- More hyperparameter tuning
- Use other classification algorithms 

