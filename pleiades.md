---
elm:
  dependencies:
    gicentre/elm-vegalite: latest

narrative-schemas:
  - pleiades
---

## Author: Yogi Kiran Jayaramu

## Ancient civilizations

Before we begin to Visualize the data, the data in the form of a csv file is explored and the Data Cleaning process is carried out to enhance the quality of data.

### Importing necessary libraries for Data preprocessing and Data visualization

```python {l}
import altair as alt
import pandas as pd
```
Reading the dataset into dataframe:
```python {l}
data = df = pd.read_csv('pleiades-locations.csv')
```
Data inconsistencies, accuracy and incompleteness of the data is handled in the Data pre-processing stage which performs Data cleaning(handling missing values, Smoothening noisy data etc), the data does not require integration with other files such as names and places as the required data(columns of interest) for visualization is available in a single file - locations. Other processes which include data reduction and data transformation is not necessary to work on Pleiades location dataset at the initial stage and therefore the transformations and filters are applied on the dataframe directly using Altair library during Visualization process.

## Data Cleaning 
 Techniques used:
        **1.** Missing data usually in the form of Nan is dropped.
        **2.** The blank data signifying null is retained on purpose which indicates unknown entries rather than missing data which may not be really usefull in Data analysis but still provides the information on unknown data.
        **3.** Noisy data such as extra spaces, commas and erroneous data is removed and handled using Pandas library.
        **4.** Filtering and Transformation is applied directly through Altair transformation techniques whereever applicable.
        **5.** Splitting aggregated data such as fields - created and modified 'time' fields is also carried out using Altair library functionalites.

```python {l}
data.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 40610 entries, 0 to 40609
Data columns (total 25 columns):
 #   Column             Non-Null Count  Dtype  
---  ------             --------------  -----  
 0   authors            40610 non-null  object 
 1   bbox               33303 non-null  object 
 2   created            40610 non-null  object 
 3   creators           40610 non-null  object 
 4   currentVersion     40465 non-null  float64
 5   description        27154 non-null  object 
 6   featureType        40574 non-null  object 
 7   geometry           33337 non-null  object 
 8   id                 40610 non-null  object 
 9   locationPrecision  40576 non-null  object 
 10  locationType       40364 non-null  object 
 11  maxDate            37638 non-null  float64
 12  minDate            37638 non-null  float64
 13  modified           40610 non-null  object 
 14  path               40610 non-null  object 
 15  pid                40610 non-null  object 
 16  reprLat            33303 non-null  float64
 17  reprLatLong        33303 non-null  object 
 18  reprLong           33303 non-null  float64
 19  tags               480 non-null    object 
 20  timePeriods        37636 non-null  object 
 21  timePeriodsKeys    37638 non-null  object 
 22  timePeriodsRange   37638 non-null  object 
 23  title              40610 non-null  object 
 24  uid                40610 non-null  object 
dtypes: float64(5), object(20)
memory usage: 7.7+ MB
```

### Basic Cleaning
The NaN values are filtered using the columns 'bbox' and 'maxDate' whose missing values cannot be replaced by an approximation as it does not add value or retain the dataset information and is better to drop tuples which has missing values for these columns.

```python {l}
data.dropna(axis=0, subset=['bbox'], inplace =True)
data.dropna(axis=0, subset=['maxDate'], inplace =True)
```

```python {l}
data.info()

<class 'pandas.core.frame.DataFrame'>
Int64Index: 31485 entries, 0 to 40609
Data columns (total 25 columns):
 #   Column             Non-Null Count  Dtype  
---  ------             --------------  -----  
 0   authors            31485 non-null  object 
 1   bbox               31485 non-null  object 
 2   created            31485 non-null  object 
 3   creators           31485 non-null  object 
 4   currentVersion     31383 non-null  float64
 5   description        26243 non-null  object 
 6   featureType        31451 non-null  object 
 7   geometry           31485 non-null  object 
 8   id                 31485 non-null  object 
 9   locationPrecision  31485 non-null  object 
 10  locationType       31242 non-null  object 
 11  maxDate            31485 non-null  float64
 12  minDate            31485 non-null  float64
 13  modified           31485 non-null  object 
 14  path               31485 non-null  object 
 15  pid                31485 non-null  object 
 16  reprLat            31485 non-null  float64
 17  reprLatLong        31485 non-null  object 
 18  reprLong           31485 non-null  float64
 19  tags               469 non-null    object 
 20  timePeriods        31483 non-null  object 
 21  timePeriodsKeys    31485 non-null  object 
 22  timePeriodsRange   31485 non-null  object 
 23  title              31485 non-null  object 
 24  uid                31485 non-null  object 
dtypes: float64(5), object(20)
memory usage: 6.2+ MB
```

The columns of interest does not contain any Nan values except for the featureType. Since the featureType is also a categorical value and since it has a minimum of 34 tuples, the values Nan for featureType can also be dropped.

```python {l}
data.dropna(axis=0, subset=['featureType'], inplace =True)
```
```python {l}
data.isnull().sum()

authors                  0
bbox                     0
created                  0
creators                 0
currentVersion         101
description           5239
featureType              0
geometry                 0
id                       0
locationPrecision        0
locationType           242
maxDate                  0
minDate                  0
modified                 0
path                     0
pid                      0
reprLat                  0
reprLatLong              0
reprLong                 0
tags                 30985
timePeriods              2
timePeriodsKeys          0
timePeriodsRange         0
title                    0
uid                      0
dtype: int64
```

**The rest of the data which forms the major bulk of the data is retained to perform Visualization**

The data limitation of 5000 rows is removed to visualize the major portion of data.

```python{l}
alt.data_transformers.enable('default', max_rows=None)

DataTransformerRegistry.enable('default')
```

## Getting started

**Selection Histogram**

{(aim|}

To identify the 'creators' and the count/number of each creators contribution in relation to the timeperiod for which the feature/location is available.
    
The scatter plot provides the distribution of features/locations with respect to the minimum date and the maximum date for which the locations existed. The tooltip provides the name of the creator of pleiades location.
      
The points can be selected, if unselected, the total counts(aggregation) of number of pleiades location is provided by the histogram. The interactive scatterplot provides an option for interval selection and the corresponding count of locations for a particular creator is displayed. The histogram retains only the selection data on scatterplot thereby providing the selection detail display.
        
The outliers of the data can be clearly seen, a location is found at a minimum date of less than -2500000 and existed till 2000.
Similarly, an another outlier can be spotted with minDate as 0 and maxDate as less than -9000.
        
The histogram data reveals - 'jbecker' to be the highest contributor as a creator followed by 'jahlfeldt'. Major contributions as creators is recorded in the locations data.

{|aim)}

```python {l}

interval = alt.selection_interval()

base = alt.Chart(data).mark_circle(size = 50).encode(
    y='minDate',
    color=alt.condition(interval, 'creators', alt.value('lightgray')),
    tooltip = 'creators'
).properties(
    selection=interval
)

hist = alt.Chart(data).mark_bar().encode(
    x='count()',
    y='creators',
    color='creators',
#     tooltip = 'creators'
).properties(
    width=800,
    height=3000
).transform_filter(
    interval
)

scatter = base.encode(x='maxDate') 
scatter.interactive() & hist

```
The interaction is shown below, the specific point is zoomed in to investigate the points without selection.
![Selectionhistogram2](Selectionhistogram2.png)

The selected points generate a histogram with the count value:

Example 1: ![Selectionhistogram1](Selectionhistogram1.png)
Example 2: ![Selectionhistogram3](Selectionhistogram3.png)
Example 3: ![Selectionhistogram4](Selectionhistogram4.png)
<!-- Example 4:
![Selectionhistogram5](Selectionhistogram5.png) -->

{(vistype|}
Selection histogram(scatter plot with tooltip and histogram based on points selection)
{|vistype)}

{(vismapping|}

```python
Base Scatter plot :
        y = minDate
        x = maxDate
        size = 50
        shape = circle
        tooltip = creators
        selection type = interval selections
        color = interval creators
 Histogram :
        y = creators
        x = aggregation of creators
        color = creators
        size = width:800, height:3000
```
{|vismapping)}

{(dataprep|}
In order to display the data, the preprocessing step required is dropping Nan values from columns of interest such as creators. Before the start of visualization, the basic cleaning step of the columns is performed to remove Nan values.
The aggregation is carried out using Altair functionality at the visualization step. Max rows of 5000 limit is removed to visualize the entire data obtained after cleaning.
{|dataprep)}

{(limitations|}
A minimap can be created which improves the interaction activity by providing zoom in data smoothly. Since the dataset is large,the interaction is slow and areas of interest can be separated with a minimap which improves interaction speed.
Multiple scatter plots with a subset of min and max Date provides better flexibility and interaction of data.
{|limitations)}
