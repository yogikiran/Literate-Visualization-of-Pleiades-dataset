---
follows: followup2
---
## Multi-Feature Scatterplot (concantenated)

The multi-feature scatter plots address the concern of restrictions provided in the previous plot by bringing multi-variate analysis and showing relationship among other variables.

Basic Feature addition by creating two columns with cleaned data 'SfeatureType' and 'SlocationType'.

```python
data['SfeatureType'] = data['featureType'].str.replace(",","")
data['SlocationType'] = data['locationType'].str.replace(",","")
```

```python
data['SfeatureType']

0          settlement
1            temple-2
2             unknown
3             unknown
4             unknown
             ...     
40605          fort-2
40606            fort
40607    amphitheatre
40608    amphitheatre
40609        temple-2
Name: SfeatureType, Length: 31451, dtype: object
```
```python
data['SlocationType']

0        representative
1        representative
2        representative
3        representative
4        representative
              ...      
40605    representative
40606    representative
40607    representative
40608    representative
40609    representative
Name: SlocationType, Length: 31451, dtype: object
```
{(aim|}
The distribution of feature types with respect to its existence - minDate and maxDate is plotted in the first chart.
The relationship of the feature type with location type can be identified with the size parameter when plotting in the scatter plot. The locations has varied feature types distributed at different maxDate and minDate. 
From the plot, it can be clearly seen the location type is majorly 'representative' and all kinds of features/locations are available as representative. The tooltip shows which feature type is located at a particular location.
The second concatenated chart derives the relation between location type and location precision. The plot clearly shows a majority amount of 'representative' location type both among rough and precision locations.
The unknown value for the location type is also retained to compare if there are major unknown location types but unknown location types are only a few.
{|aim)}

{(vistype|}
Multi-Feature Scatterplot concantenated
{|vistype)}

```python
interval = alt.selection_interval()

repr1 = alt.Chart(data).mark_circle().encode(
    alt.X('minDate', scale=alt.Scale(zero=False)),
    alt.Y('maxDate', scale=alt.Scale(zero=False, padding=1)),
    color='SfeatureType',
#     color=alt.condition(interval, 'Sfeaturetype:N', alt.value('lightgray')),
    size='SlocationType',
    tooltip = 'SfeatureType:N'
).interactive()

repr2 = alt.Chart(df).mark_circle().encode(
    alt.X('minDate', scale=alt.Scale(zero=False)),
    alt.Y('maxDate', scale=alt.Scale(zero=False, padding=1)),
    color='SlocationType',
    size='locationPrecision',
    tooltip = 'SlocationType'
).interactive()

repr1
```

```python
repr2
```
Example 1
First half plot
![multifeaturescatter1](multifeaturescatter1.png)

Second half plot
![multifeaturescatter2](multifeaturescatter2.png)

Tooltip and interaction is shown in the below plots.
First half plot examples
Example 2.1
![multifeaturescatter5](multifeaturescatter5.png)

Example 2.2
![multifeaturescatter6](multifeaturescatter6.png)

Example 2.3
![multifeaturescatter7](multifeaturescatter7.png)

Second half plot examples
Example 2.1.1
![multifeaturescatter3](multifeaturescatter3.png)

Example 2.1.2
![multifeaturescatter4](multifeaturescatter4.png)

Example 2.1.3
![multifeaturescatter8](multifeaturescatter8.png)

{(vismapping|}
```python
    chart1:
        x: minDate
        y: maxDate
        color: featureType
        size: locationType
        tooltip: featureType
    chart2:
        x: minDate
        y: maxDate
        color: locationType
        size: locationPrecision
        tooltip: locationType
    Interactive is set to both the charts
```
{|vismapping)}

{(dataprep|}
'FeatureType' and 'locationType' Nan values are dropped at the initial stage of cleaning. Feature addition of columns is performed to obtain a new column SfeatureType with the cleaning of redundant commas with repeated entries.
Similarly, the SlocationType is cleaned with redundant values. The data is stripped to retain required values.
{|dataprep)}

{(limitations|}
The count of each featuretypes can be computed and an histogram with major features along with its relation to locationtype can be created which provides more information on the numbers of each feature types and its relation to locationtype.
{|limitations)}
