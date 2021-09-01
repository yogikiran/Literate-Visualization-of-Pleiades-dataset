---
follows: followup3
---
## Maps

Put your final design and discussion here

{(aim|}
The latitude and longitude values are used to plot the points on the geographic map. The plots represent the feature type on the map.
The point display is made interactive with the display based on interactive legend.
The specific feature type can be selected on the legend and the points corresponding to the legend is displayed on the geographic map.
The tooltip is provided for the feature type. Different points such as 'bath' displayed in yellow colour are found only in European regions, the feature types such as 'abbey' is found in african and Asian places.
The 'water-open' areas in the Indian ocean and islands in the atlantic ocean are found.
{|aim)}

{(vistype|}
Map
{|vistype)}

```python
countries = alt.topo_feature(data.world_110m.url, 'countries')

selection = alt.selection_multi(fields=['SfeatureType'], bind='legend')

background = alt.Chart(countries).mark_geoshape(
    fill='lightgray',
    stroke='white'
).project(
    "equirectangular"
).properties(
    width=1200,
    height=1000
)

points = alt.Chart(df_locations_mod_temp).mark_circle().encode(
    longitude='reprLong:Q',
    latitude='reprLat:Q',
    color='SfeatureType:N',
    size=alt.value(50),
    tooltip='SfeatureType:N',
    opacity=alt.condition(selection, alt.value(1), alt.value(0))
).add_selection(
    selection
)

background + points
```
Example 1 - Map displaying all the feature points
![map1](map1.png)

**Using Interactive legend**
Example 2 - Displaying "arch" values 
![map2](map2.png)

Example 3 -Displaying "abbey" values
![map3](map3.png)

Example 4 -Displaying "Basilica" values
![map4](map4.png)

Example 4 -Displaying "bath" values
![map5](map5.png)

**Showing Tooltip functionality**
Example 5 - Showing tooltip value "island"
![map6](map6.png)

Example 6 - Showing tooltip value "river"
![map7](map7.png)

{(vismapping|}
```python
background:
        project: equirectangular
        fill: lightgray
        stroke: white
    points:
        longitude=reprLong
        latitude=reprLat
        color=featureType
        size=50
        tooltip=featureType
```
{|vismapping)}

{(dataprep|}
Basic filtering of data which requires cleaning of Nan data is carried out on feature type and latitude/longitude values.
The unknown values for the feature type is retained to plot unknown values on the map thereby, retaining the distribution of data on the map and not only plotting identified areas.
{|dataprep)}

{(limitations|}
The map does not provide zoom in functionality to focus on required areas. The zoom in interaction facility can be provided which provides the detailed display.
{|limitations)}
