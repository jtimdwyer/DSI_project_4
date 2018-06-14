# DSI Project 4 - Predicting West Nile Virus in Chicago

## Table of Contents

1. Background
2. Data Cleaning
3. Exploratory Data Analysis
4. Modeling
5. Conclusions

## Background

The goal of this project was to predict the occurrence of West Nile Virus (WNV) in Chicago, IL. We obtained the data from a [Kaggle Competition](https://www.kaggle.com/c/predict-west-nile-virus/) We obtained three data sets for this project: a dataset marking the traps which were placed around Chicago to monitor mosquitos and check for the presence of West Nile Virus (split into a training set and a testing set), a dataset containing information about the weather occurring in the months when the mosquito data was being collected, and a dataset indicating the dates and locations where pesticides were sprayed by the city to eliminate adult mosquitos. Our goal is to perform analysis on the provided data and create machine learning models that predict the presence of West Nile Virus in a given mosquito trap in Chicago.

<<<<<<< HEAD
<<<<<<< HEAD
This project is based off of a Kaggle competition, with the directive being to predict West Nile Virus occurrances in mosquitos across the city of Chicago. We received three data sets for this project: a dataset marking the traps which were placed around Chicago to monitor mosquitos and check for the presence of West Nile Virus (split into a training set and a testing set), a dataset containing information about the weather occurring in the timeframe in which the mosquito data was being collected, and a dataset indicating the dates and locations where airborne pesticides were sprayed by the city. Our goal is to perform analysis on the data given, and create a model to accurately predict the presence of West Nile Virus in a given mosquito trap in Chicago.
=======
## Data Cleaning
>>>>>>> 28e82e536fc4d2efa6b1a4364bf9d84ca21b1aca
=======
## Data Cleaning
>>>>>>> 39e94e554aea95af69f8d3387faecf2134553848

### Training Data

The training data contained no missing data. We collapsed the number of mosquito species by putting all the species of mosquitos who were infrequent into a category named "Culex Other." (The infrequent species together totaled ~3% of the entries) We chose to inpute any unspecified mosquitos with the most common species in the Chicago region, C. pipiens based on data presented [here](http://www.ajtmh.org/content/journals/10.4269/ajtmh.2009.80.268#html_fulltext) by Hamer et. al: 

### Weather Data

Null values in the weather data were encoded as "M" or "-". Where the number of "M"s was low, we dropped the missing entries (`Tavg`, `WetBulb`, `Depth`). When the number of missing values for a feature was high, we dropped the feature column (`Depart`, `Sunrise`, `Sunset`). A couple of columns required more consideration; for `PrecipTotal`, a measure of the total precipitaion for a day, some values were imputed as "T," for Trace (stated in documentation). We imputed 0.01 in these entries, an estimate for what a trace amount of rainfall would be.  The `CodeSum` column showed the weather conditions for a given day. Because some entries in this column contained more than one condition, we parsed the column into separate columns coding for the given weather conditions individually. An analog to creating dummy variables, we took this apprach to cut down on the number of features our end dataset would contain.

### Spray Data

The `Time` column of the Spray dataset had many null values, so we chose to drop the column. We removed a number of duplicate latitude/longitude coordinates, assuming these to be an imputation error because the entries themselves were duplicates. We then wrote a function that returned a variable `spray_nearby` which indicated whether a trap was within 1/8th of a mile of a sprayed zone.

### Data Merging

In order to merge the weather data and the training data, we engineered a feature `station` which indicated the closest station to a trap. We then merged the two datasets on the corresponding dates and columns. We merged the spray data in on the union of our trap ids and the dates.

## Exploratory Data Analysis

<<<<<<< HEAD
<<<<<<< HEAD
Speaking on the test dataset provided by Kaggle, we spent time adjusting the testing data to match up with our set of training data. This process entailed merging the dataset with our weather and spray features, and accounting for any variables not represented in the training data.
=======
We created a number of plots to help us visually interpret our data (found in the EDA notebook). By graphing the cases of West Nile Virus against the latitude of the traps, we determined that there were more cases of the West Nile Virus on the north side of the city. Looking at weather data plotted against the presence of West Nile Virus, mosquitos were present more in days with hotter temperatures, and higher humidities. 
>>>>>>> 39e94e554aea95af69f8d3387faecf2134553848

## Modeling

<<<<<<< HEAD
We created a number of plots to help us visually interpret our data. By graphing the cases of West Nile Virus against the latitude of the traps, we determined that there were more cases of the West Nile Virus on the north side of the city. Looking at weather data plotted against the presence of West Nile Virus, the virus,mosquitos were present more in days with hotter temperatures, and higher humidities. 
=======
We created a number of plots to help us visually interpret our data. By graphing the cases of West Nile Virus against the latitude of the traps, we determined that there were more cases of the West Nile Virus on the north side of the city. Looking at weather data plotted against the presence of West Nile Virus, mosquitos were present more in days with hotter temperatures, and higher humidities. 
>>>>>>> 28e82e536fc4d2efa6b1a4364bf9d84ca21b1aca
=======
We tested several variations of machine learning models in an effort to maximize the receiver operating characteristic area under the curve (ROC AUC). The ROC AUC is a measure of the ability of a classifier to distinguish between two or more classes; a classifier that randomly guesses (for binary classification) will achieve a ROC AUC of 0.5; one that classifies perfectly will score 1.0. The table below lists a selection of the tested models and their corresponding ROC AUC scores:
>>>>>>> 39e94e554aea95af69f8d3387faecf2134553848



Our main modeling strategy was to vary the size of the feature space of the model inputs. We tested models using the entire feature space, a selection of features that we believed we were most predictive of WNV presence, and using a feature space of principal components generated by Principal Component Analysis (PCA). Overall, reducing the number of features (through either manual feature selection or PCA) resulted in the greatest improvements of our metric.

## Conclusions


