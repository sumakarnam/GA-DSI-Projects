# Project 2 Ames Housing Prediction 

## Contents

- <a href='#background'>Background and Problem Statement</a>
- <a href='#part1'>Part1</a>
    - Functions
    - Data Import & Cleaning
    - Exploratory Data Analysis
- <a href='#part2'>Part2</a>
    - Feature Engineering
    - Model fitting, cross validation
- [Conclusions and Further Improvements to the model](#Conclusions-and-Recommendations)

<a id='background'></a>
## Background and Problem Statement

The goal of this analysis is to predict housing prices in Ames, Iowa using extensive [data](../datasets/train.csv) about various features of the house (79 features about the size, quality, amenities etc.). A detailed description of the dataset can be found in the [data description](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt)

The house prices are predicted using a Linear Regression model and the model was tuned to minimize root mean square error on the available data.

Problem Statement: As the observed house prices have a log normal distribution, in addition to predicting the house prices for the 878 houses in the [test data](../datasets/test.csv), this analysis aims to determine if predicting log prices instead of prices gives a better fitting model and to quantify the lift in R<sup>2</sup> if this is indeed the case.

<a id='Part1'></a>
## Part 1

The analysis can be broadly divided into two parts. The first part contains the following sections

- Functions
- Data Import & Cleaning
- Exploratory Data Analysis

Part 1 begins with defintions of some plotting and scoring functions used repeatedly for making visualisations and scoring the models, followed by importing the dataset and treating columns with few missing values by either 
replacing them with the category mode or column mean for categorical and numerical variables respectively. This is followed by an extensive EDA of most of the columns in the provided dataset to inform feature selection



<a id='Part2'></a>
## Part 2

The second part contains the following sections

- Feature Engineering
- Model fitting, cross validation 

The Feature Engineering section processes the data to be fed into the model based on the feature selection choices made in EDA. The data is first split randomly into a training set and test set and is processed for the two base models - one for predicting home sale price and another for predicting log of the sale price. 

These models had very similar performance, with the log price model outperforming the price model marginally on the test data (R<sup>2</sup> of 0.81 vs 0.82 on the test data). The RMSE on this particular training data was much higher for the log Price model compared to the price model (44008 vs 36203). This is likely an effect of some variance in the model and higher representation of some categories in the train dataset.

Two other model iterations have been attempted:
- As Neighborhood turned out to be a feature with a fairly high coefficient in the base models, in an attempt to capture the effect of neighborhood better, a new feature (Area of the home * average price per sqft) was engineered and used in a new model
-Another model iteration was done with the addition of an interaction feature (Area * #Rooms) in place of the Area and #Rooms features. 

## <a id='Conclusions-and-Recommendations'></a>
## Conclusions and Future Model Improvements

- Conclusions
    - The various iterations did not make a considerable difference to the model performance from the base model with R<sup>2</sup> of 0.81
    - The mean encoded variables generally have the highest coefficients as expected across all the models
    - The performance of the model with the mean encoding of the selected variables was considerably higher than when these variables were one-hot encoded (This analysis is not included in the report)
- Further Improvements
    - In cases where one of the features was dropped in favor of another due to high correlation between the two,
    it will be interesting to see the effect of building a model with the dropped feature instead of the feature that was used (example: 'Garage Area' vs 'Garage Cars')
    - Creating more interaction terms by using the product of the two highly correlated similar variables and analysing the effect on the model (example: 'Gr Liv Area' vs '1st Flr SF' and '2nd Flr SF')
    - Building separate models for various groups of Neighborhoods
    - Investigate other ways to address multicollinearity

