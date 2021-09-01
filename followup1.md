---
follows: pleiades
---

## Streamgraph

{(aim|}

The concentration of locations creation or modfications done is recorded as a stream graph with changing year and months.
The zooming or interaction facility provides the option to check the concentration of changes done or the creation of locations/features. The charts are placed side by side instead of overlay to reduce clutter and obtain clear display of creation and modification years.

From an immediate look, the Created year 2011 to 2012 has major locations/features identified, there is a significant drop in identifying more locations moving down the years. The modifications is majorly placed in the year 2012. There is a huge concentration of activity for both creation and modification in the year 2011-2012. 
Moving down further years, the creation of locations has reduced dramatically with only few features/locations identifed throughout the year, the modifications peak can be found again in the year 2018-2019. At the present moment, there is only a minimum amount of creations and modifications done to the dataset.

The peak years for the creation and modifications lasted only for a couple of years 2011-2012. Creation started from the year 2009 and modifications only started after 2011 when there was also a good amount of creations parallely being done.

The hover functionality highlights the location precision values to identify whether the creations/modifications done are either rough or precise. From the visualization it can be easily verified there are more creations of precise locations when compared to rough estimation. The modification stream also provides a similar data where the precise locations are modified more compared to roughly identified locations.

{|aim)}

The feature addition columns are created to perform aggregate operations.

```python
df['yearC'] = [str(x).split('-')[0] for x in df.iloc[:,2]]
df['yearM'] = [str(x).split('-')[0] for x in df.iloc[:,13]]
```

```python
df['yearC'].head()

0    2013
1    2013
2    2011
3    2010
4    2011
Name: yearC, dtype: object
```

```python
df['yearM'].head()

0    2016
1    2016
2    2011
3    2012
4    2012
Name: yearM, dtype: object
```

```python
hover = alt.selection_single(on='mouseover')

S1 = alt.Chart(data).mark_area().encode(
    alt.X('year(created):T', timeUnit='yearmonth',
      axis=alt.Axis(format='%Y', labelAngle=0, title='year')),
    alt.Y('sum(yearC):Q',
        stack='center'
    ),
    color=alt.Color('locationPrecision:N', scale=alt.Scale(scheme="category20b")),
    opacity=alt.condition(hover, alt.value(1.0), alt.value(0.5))
).properties(
    selection=hover
).interactive()

S2 = alt.Chart(data).mark_area().encode(
    alt.X('year(modified):T', timeUnit='yearmonth',
      axis=alt.Axis(format='%Y', labelAngle=0, title='year')),
    alt.Y('sum(yearM):Q',
        stack='center'
    ),
    color=alt.Color('locationPrecision:N', scale=alt.Scale(scheme="category20b")),
    opacity=alt.condition(hover, alt.value(1.0), alt.value(0.5))
).properties(
    selection=hover
).interactive()

S1 | S2
```
Created year and Modified year plots arranged side by side.
![Stream1](Stream1.png)

The zoom in interactive functionality is shown as below:
![Stream2](Stream2.png)

The hover functionality highlighting mouse on stream graph
![Stream3](Stream3.png)

Hover and zoom in functionality combined: 
![Stream4](Stream4.png)


{(vistype|}
Stream graph (Vertically stacked)
{|vistype)}


{(vismapping|}

```python
Stream graph1:
        x : Created year
        y : aggregation on created year
        color : Location Precision
        opacity : hover on mouseover for location precision
        scale : default, scheme - category20b
Stream graph2:
        x : modified year
        y : aggregation on modified year
        Color : Location Precision
        opacity : hover on mouseover for location precision
        scale : default, scheme - category 20b
```

{|vismapping)}

{(dataprep|}
Feature addition for created and modified is carried out to strip yearC and yearM to perform aggregation operation on this columns. Nan values are ensured to be dropped for created and modified fields. Location precision is also ensured to contain non-null values.
{|dataprep)}

{(limitations|}
A better drill down display with the consideration of time and months can be done with the help of the slider functionality which provides the filtering of data at particular points and measure the exact count with the tooltip functionality.
Heatmaps can also be created highlighting the concentrated areas of creation and modification.
{|limitations)}
