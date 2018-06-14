# DSI Project 4 - Predicting West Nile Virus in Chicago

## Table of Contents

1. Background
2. Data Cleaning
3. Exploratory Data Analysis
4. Modeling
5. Conclusions

## Background

The goal of this project was to predict the occurrence of West Nile Virus in Chicago, IL. We obtained the data from a [Kaggle Competition](https://www.kaggle.com/c/predict-west-nile-virus/) We obtained three data sets for this project: a dataset marking the traps which were placed around Chicago to monitor mosquitos and check for the presence of West Nile Virus (split into a training set and a testing set), a dataset containing information about the weather occurring in the timeframe in which the mosquito data was being collected, and a dataset indicating the dates and locations where airborne pesticides were sprayed by the city to eliminate adult mosquitos. Our goal is to perform analysis on the data given, and create machine learning models to accurately predict the presence of West Nile Virus in a given mosquito trap in Chicago.

## Data Cleaning

### Training Data

The training data contained no missing data. We collapsed the number of mosquito species by putting all the species of mosquitos who were infrequent into a category named "Culex Other." (The infrequent species together totaled ~3% of the entries) We chose to inpute any unspecified mosquitos with the most common species in the Chicago region, C. pipiens based on data presented [here](http://www.ajtmh.org/content/journals/10.4269/ajtmh.2009.80.268#html_fulltext) by Hamer et. al: 

### Weather Data

Null values in the weather data were encoded as "M" or "-". Where the number of "M"s was low, we dropped the missing entries (`Tavg`, `WetBulb`, `Depth`). When the number of missing values for a feature was high, we dropped the feature column (`Depart`, `Sunrise`, `Sunset`). A couple of columns required more consideration; for `PrecipTotal`, a measure of the total precipitaion for a day, some values were imputed as "T," for Trace (stated in documentation). We imputed 0.01 in these entries, an estimate for what a trace amount of rainfall would be.  The `CodeSum` column showed the weather conditions for a given day. Because some entries in this column contained more than one condition, we parsed the column into separate columns coding for the given weather conditions individually. An analog to creating dummy variables, we took this apprach to cut down on the number of features our end dataset would contain.

### Spray Data

The `Time` column of the Spray dataset had many null values, so we chose to drop the column. We removed a number of duplicate latitude/longitude coordinates, assuming these to be an imputation error because the entries themselves were duplicates. We then wrote a function that returned a variable `spray_nearby` which indicated whether a trap was within 1/8th of a mile of a sprayed zone.

### Data Merging

In order to merge the weather data and the training data, we engineered a feature `station` which indicated the closest station to a trap. We then merged the two datasets on the corresponding dates and columns. We merged the spray data in on the union of our trap ids and the dates.

### Exploratory Data Analysis

We created a number of plots to help us visually interpret our data. By graphing the cases of West Nile Virus against the latitude of the traps, we determined that there were more cases of the West Nile Virus on the north side of the city. Looking at weather data plotted against the presence of West Nile Virus, mosquitos were present more in days with hotter temperatures, and higher humidities. 

#### Modeling

We tested a number of models with the goal of predicting whether these mosquito traps registered the West Nile Virus, in the even years not provided to us. Our metric for scoring our model was the ROC-AUC curve. The ROC-AUC curve is a plot of the true positive rate against the false positive, used to determine the accuracy of the model. 


