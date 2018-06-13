# DSI_project_4
DSI project 4 - West Nile Virus

### Table of Contents:

1. Background
2. Data Cleaning
3. Exploratory Data Analysis
4. Modeling
5. Conclusions


#### Background

This project is based off of a Kaggle, with the directive being to predict West Nile Virus occurrances in mosquitos across the city of Chicago. We received three data sets for this project: a dataset marking the traps which were placed around Chicago to monitor mosquitos and check for the presence of West Nile Virus (split into a training set and a testing set), a dataset containing information about the weather occurring in the timeframe in which the mosquito data was being collected, and a dataset indicating the dates and locations where airborne pesticides were sprayed by the city. Our goal is to perform analysis on the data given, and create a model to accurately predict the presence of West Nile Virus in a given mosquito trap in Chicago.

#### Data Cleaning

##### Training Data

The training data provided by Kaggle contained no missing data. We collapsed the number of mosquito species, by putting all the species of mosquitos who were infrequent into a category named "Culex Other." (The infrequent species together totaled ~3% of the entries)

##### Weather Data

There were null entries in our Weather data, encoded as "M" or "-" for missing. In columns where the number of "M"s was low, we dropped the entries (Tavg, WetBulb, Depth). When the number of missing values in a column was high, we dropped the column (Depart, Sunrise, Sunset). A couple of columns required more consideration; for PrecipTotal, a measure of the total precipitaion for a day, some values were imputed as "T," for Trace (stated in documentation). We imputed 0.01 in these entries, an estimate for what a trace amount of rainfall would be.  The CodeSum column showed the weather conditions for a given day. Because some entries in this column contained more than one condition, we parsed the column into separate columns coding for the given weather conditions individually. An analog to creating dummy variables, we took this apprach to cut down on the number of features our end dataset would contain.

##### Spray Data

The Time column of the Spray dataset had many null values, so we dropped the column. We removed a number of duplicate latitude/longitude coordinates, assuming these to be an imputation error because the entries themselves were duplicates. We then wrote a function that returned a variable which indicated whether a trap was within 1/8th of a mile of a sprayed zone.

##### Data Merging

In order to merge the weather data and the training data, we engineered a feature which indicated the closest station to a trap. We then merged the two datasets on the corresponding dates and columns. We merged the spray data in on the union of our trap ids and the dates.

##### Exploratory Data Analysis

We created a number of plots to help us visually interpret our data. By graphing the cases of West Nile Virus against the latitude of the traps, we determined that there were more cases of the West Nile Virus on the north side of the city. Looking at weather data plotted against the presence of West Nile Virus, the virus,mosquitos were present more in days with hotter temperatures, and higher humidities. 


