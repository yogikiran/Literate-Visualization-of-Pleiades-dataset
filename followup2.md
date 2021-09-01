---
follows: followup1
---
## Dot-Dash plot

The dot-dash plot can be used as a simple form of interactive graph yet providing useful information on the distribution and variability of a variable.

{(aim|}
To identify the distribution of precise and rough estimation of locations with respect to date of existence(min and max Date).
The concentration of rough locations identified shows that the roughly estimated places are not purely because of its existence from a long time. The rough location precision for a feature/location is found for max Date of 2000 with min Date of less than -2600000.
The ticks also provide the information that the rough location precision existed for a max Date ranging from -2000 to 2000. The precisely identified locations are large and the min Date is only after -200000 for precisely identified locations and it has lasted longer than roughly identified places i.e., more than 2000 as max Date
{|aim)}

{(vistype|}Dot-Dash plot
{|vistype)}

```python

brush = alt.selection(type='interval')

tick_axis = alt.Axis(labels=False, domain=False, ticks=False)
tick_axis_notitle = alt.Axis(labels=False, domain=False, ticks=False, title='')

points = alt.Chart(data).mark_point().encode(
    x=alt.X('minDate:Q', axis=alt.Axis(title='')),
    y=alt.Y('maxDate:Q', axis=alt.Axis(title='')),
    color=alt.condition(brush, 'locationPrecision:N', alt.value('grey')),
    tooltip='locationPrecision:N'
).properties(
    selection=brush
)

x_ticks = alt.Chart(data).mark_tick().encode(
    alt.X('minDate:Q', axis=tick_axis),
    alt.Y('locationPrecision:N', axis=tick_axis_notitle),
    color=alt.condition(brush, 'locationPrecision:N', alt.value('lightgrey'))
).properties(
    selection=brush
)

y_ticks = alt.Chart(data).mark_tick().encode(
    alt.X('locationPrecision:N', axis=tick_axis_notitle),
    alt.Y('maxDate:Q', axis=tick_axis),
    color=alt.condition(brush, 'locationPrecision:N', alt.value('lightgrey'))
).properties(
    selection=brush
)

y_ticks | (points.interactive() & x_ticks)
```
Dot-dash plot without interaction

Example 1
![Dotdash2](Dotdash2.png)

Example 2
![Dotdash3](Dotdash3.png)

Dot-dash plot showing interactions of the data and ticks

Example 3
![Dotdash1](Dotdash1.png)

Example 4
![Dotdash4](Dotdash4.png)

Dot-dash plot showing tooltip functionality

Example 5
![Dotdash5](Dotdash5.png)

Example 6
![Dotdash6](Dotdash6.png)

{(vismapping|}
```python
points on scatter:
        x: minDate
        y: maxDate
        color: locationPrecision
        tooltip: locationPrecision
        selection: brush (interval selection)
        interactive is set
x-ticks:
        x: minDate
        y: locationPrecision
        color: locationPrecision
        selection: brush (interval selection)
y-ticks:
        x: locationPrecision
        y: maxDate
        color: locationPrecision
        selection: brush (interval selection)
```
{|vismapping)}

{(dataprep|}
No additional datapreparation is used. only the cleaning of Nan values for minDate, maxDate and locationPrecision.
{|dataprep)}

{(limitations|}
Relationship among multiple parameters are not plotted and therefore the relationship is restricted to locationPrecision thereby, not providing multivariate analysis. The plot can be extended to provide multivariate analysis.
{|limitations)}
