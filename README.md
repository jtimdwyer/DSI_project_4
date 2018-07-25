#  Predicting West Nile Virus in Chicago
_By Jordan Bailey, George Dewey and Tim Dwyer_

## Table of Contents

1. Background
2. Data Cleaning
3. Exploratory Data Analysis
4. Modeling
5. Conclusions

## Background

The goal of this project was to predict the occurrence of West Nile Virus (WNV) in Chicago, IL. We obtained data from the City of Chicago hosted as part of a Kaggle competition [here.](https:/www.kaggle.com/c/predict-west-nile-virus/) The data was obtained in 3 separate files as described below: 

1) A dataset containing location data for traps placed around Chicago to monitor mosquito populations. Samples from these traps were collected by city workers and tested for the presence of WNV.
2) A dataset containing information about the weather occurring in the months when the mosquito data was being collected.
3) A dataset indicating the dates and locations where pesticides were sprayed by the city to eliminate adult mosquitos. 

Our goal was to perform analysis on the provided data and create machine learning models that predict the presence of West Nile Virus in mosquito samples. We used data from 2007, 2009, 2011, and 2013 to train the models; model performance was assessed using data collected in 2008, 2010, 2012, and 2014.

## Data Cleaning

### Trap Data

The training dataset contained no missing values. We collapsed the number of mosquito species by combining species who were infrequent into a category named "Culex Other." (The infrequent species accounted for only 3% of all observations in the training set.) We chose to impute the most common species in the Chicago region, C. pipiens, for any observations marked as 'Unspecified' based on rationale presented [here](http://www.ajtmh.org/content/journals/10.4269/ajtmh.2009.80.268#html_fulltext) by Hamer et. al.

### Weather Data

Null values in the weather dataset were encoded as "M" or "-". For features where the occurence of "M"s/"-"s was low (<5%), we dropped the missing entries (`Tavg`, `WetBulb`, `Depth`). When the number of missing values for a feature was high, we dropped the feature column (`Depart`, `Sunrise`, `Sunset`). A couple of columns required more consideration; for `PrecipTotal`, the measure of the total precipitation for one day in inches, some values were encoded as "T"s, representing trace amounts of rain or snow (stated in documentation). We imputed a value of 0.01" for these trace entries.  The `CodeSum` column showed the weather conditions for a given day; because some entries in this column contained multiple codes each describing a separate weather condition, we parsed the feature into unique features for each individual code.
 
### Spray Data

The `Time` column of the Spray dataset had many null values (over 500), so we chose to drop the column. We removed instances of duplicate entries attirbuting them to a data entry or collection error. We then developed a function that created a variable `spray_nearby` which indicated whether individual traps were within 1/8th of a mile of a sprayed zone.

### Data Merging

In order to merge the weather data and the trap data, we engineered a feature `station` which encoded for the closest weather station to a trap. We then merged the two datasets using the `date` and `station` columns. We then merged the combined weather/trap dataset with our engineered spray variable using `date` and `trap`.

## Exploratory Data Analysis

We created a number of plots to visually inspect our data, found in the EDA notebook. By graphing the cases of West Nile Virus against the latitudes where traps were placed, we determined that there were more cases of the West Nile Virus on the north side of the city. Inspection of weather data plotted against the presence of WNV suggested that mosquitos with WNV were more likely to be trapped on days with higher average temperatures and lower humidities. 

## Modeling

We tested several variations of machine learning models in an effort to maximize the receiver operating characteristic area under the curve (ROC AUC). The ROC AUC is a measure of the ability of a classifier to distinguish between two or more classes; a classifier that randomly guesses (for binary classification) will achieve a ROC AUC of 0.5; one that classifies perfectly will score 1.0. The table below lists a selection of the tested models and their corresponding ROC AUC scores:

model|Kaggle Private Score|Kaggle Public score | Local Training Score | Local Validation Score
--|--|--|--|--|
Logistic Regression| .5| .5| .72451| .69808
Logistic Regression (w/PCA)|0.55512 |.55122 | .69745| .71369
AdaBoost Logistic Regression| .51699|.52268 | .69758 | .71585
KNN |.5| .5 | .76534 | .65787
Decision Tree|.5|.5 | .82956 | .66636
Decision Tree (restricted features)|.5|.5 | .72648 | .69755
Random Forest (restricted features)|.50644|.49996 | .92380| .78396

Our primary strategy for model optimization was to vary the size of the feature space of the model inputs. We tested models using the entire feature space, a selection of features that we believed we were most predictive of WNV presence, and using a feature space of principal components generated by principal component analysis (PCA). Overall, reducing the number of features (through either manual feature selection or PCA) resulted in the greatest increases in ROC AUC.

## Conclusions

The huge disparity between our local scores and the Kaggle scores indicate that our models are almost entirely modeling noise. It is our recommendation that the project continue in one or more of the following ways:

1. Restructure the data
> Since our local score are so different than the Kaggle scores, we are likely just modeling the noise present in the data. More work should be done to address this. For example we might try to adjust the way the weather data is taken into account for each observation. Currently we are assigning weather features to a given trap based on which airport it is closest to. This could be causing an issue as many of the traps are not particularly close to either airport. One approach would be to simply average the weather information, perhaps weighted by distance to the given source of the measurement. Another option would be just to add the weather information for both airports to each of the observations, completely ignoring the distance to the source.

1. Get more, cleaner data
> There is a fair amount of missing data in the weather information, so it would probably be worthwhile to track down the true missing values, if they can be found. 
 
1. Use the spray data as an intermediary step rather than a feature
> Currenlty we are using the spray data as an engineered feature indicating whether a trap has ever been sprayed (we consider a trap to have been sprayed if there is a spray location within an eighth of a mile). This is certainly straightforward but possibly too coarse and could be a large source of noise. Alternatively we could, as an intermediary step, create a classificaiton model that tries to predict whether or not a trap was sprayed on other days based on the weather. This would also run the risk of being very noisy.


## Recommendations 

Until such time as a more suitable candidate can be constructed we suggest using the provided Logistic Regression (w/PCA) model for deciding if a trap should be sprayed. Beyond that, we strongly believe that further investigation of different methods for modeling this data should yield much better results. This model is a logistic regression model using 7 features created by principal component analysis This model not only resulted in the highest ROC AUC when predicting WNV presence for the test data but also was incredibly fast to fit owing to its low feature count. 
