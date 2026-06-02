# Mapping and GeoPandas

 -- Dr. Eric Godat
 
So far, we have explored data using tables, filtering, sorting, summary
statistics, and plots.

But many datasets also have a geographic component. Maps can reveal
patterns that are difficult to see in a table or a scatterplot.

In this notebook, we will use **GeoPandas** to:

- load geographic data
- inspect geometry columns
- calculate areas, centroids, and distances
- create buffers
- test spatial relationships
- merge geographic data with ACS data
- map missing data
- create choropleth maps

The goal is not to become a GIS expert today. The goal is to understand
that **location is data**.

# Setup


```python
import pandas as pd
import geopandas as gpd
import matplotlib.pyplot as plt

%matplotlib inline
%config InlineBackend.figure_format = "retina"
```

# 1. Reading Geographic Data

GeoPandas works a lot like pandas, but it adds special support for
geographic shapes.

Those shapes are stored in a special column called `geometry`.

Today we will use a GeoJSON file containing Texas county boundaries.


```python
import shapely
import numpy as np
import fiona
counties = gpd.read_file("data/tx_counties.geojson", engine="fiona")
```

If you get a If you receive a geometry-related error, update geopandas/shapely using the code below and restart the kernel.
```
pip install --upgrade geopandas shapely
```

This will upgrade the version of these packages in our environment to a newer version and resolve our error.


```python
counties.head()
```

The full dataset may contain more columns than we need. Let’s look at
just the most useful ones.


```python
counties[["COUNTY", "FIPS", "SQUARE_MIL", "geometry"]].head()
```

Each row is one county.

The `geometry` column contains the county shape.

# 2. Our First Map

GeoPandas can draw the shapes in the geometry column directly.


```python
counties.plot(figsize=(10, 10));
```

That single line made a map.

This is the basic idea of GeoPandas:

> pandas dataframe + geometry column = mappable data

# 3. Geometry as Data

The geometry column is not just for drawing maps. We can calculate
things from it.

For example, we can calculate:

- area
- boundaries
- centroids
- distances
- buffers
- intersections

## Area

The dataset already includes a `SQUARE_MIL` column, but GeoPandas can
also calculate area from the geometry.


```python
counties["geometry_area"] = counties.area
```


```python
counties[["COUNTY", "SQUARE_MIL", "geometry_area"]].head()
```

Map projections affect area calculations, so `geometry_area` is not
always in units that are easy to interpret.

If we need to calculate area, we first project the data into a coordinate system designed for Texas: EPSG:3083.


```python
counties_projected = counties.to_crs("EPSG:3083")
```


```python
counties_projected["area_sq_miles"] = (
    counties_projected.geometry.area / 2589988.11 #1 square mile = 2,589,988.11 square meters
)
```


```python
counties_projected[["COUNTY", "SQUARE_MIL","area_sq_miles"]].sort_values(
    by="SQUARE_MIL",
    ascending=False
).head()
```

For today, we will use the provided `SQUARE_MIL` column when
we want county area in square miles.


```python
counties.sort_values(by="SQUARE_MIL", ascending=False)[["COUNTY", "SQUARE_MIL"]].head(10)
```

## Discussion

Which counties are physically largest?

Are those also the most populated counties?

# 4. Boundaries and Centroids

GeoPandas can extract the boundary of each polygon.


```python
counties_projected["boundary"] = counties_projected.boundary
```

It can also calculate the centroid, or geometric center, of each county.


```python
counties_projected["centroid"] = counties_projected.centroid
```


```python
counties_projected[["COUNTY", "boundary", "centroid"]].head()
```

Let’s plot county boundaries and county centroids together.


```python
ax = counties_projected.plot(color="white", edgecolor="black", figsize=(10, 10))
counties_projected.set_geometry("centroid").plot(ax=ax, color="red", markersize=5);
```

## Discussion

Is the geographic center of a county always where people live?

Why might centroids be useful anyway?

# 5. Distance

We can measure distance between geometries.

Let’s pick Dallas County and calculate the distance from every county
centroid to the Dallas County centroid.


```python
dallas_centroid = counties_projected.loc[counties_projected["COUNTY"] == "Dallas County", "centroid"].iloc[0]
```


```python
counties_projected["distance_to_dallas"] = counties_projected["centroid"].distance(dallas_centroid)
```


```python
counties_projected.sort_values(by="distance_to_dallas")[["COUNTY", "distance_to_dallas"]].head(10)
```

The distance values depend on the coordinate system of the file, so we
will treat them as useful for comparison rather than as precise miles.


```python
counties_projected.plot(column="distance_to_dallas", legend=True, figsize=(10, 10));
```

## Discussion

What pattern do you expect to see before running the map?

Does the map match your intuition?

# 6. Buffers

A buffer creates an area around a geometry.

You can think of this as asking:

> What is within some distance of this place?



```python
dallas_geometry = counties_projected.loc[counties_projected["COUNTY"] == "Dallas County", "geometry"].iloc[0]
```


```python
dallas_buffer = dallas_geometry.buffer(100000) # EPSG:3083 uses meters, so 100 km = 100,000 meters
```


```python
ax = counties_projected.plot(color="white", edgecolor="black", figsize=(10, 10))
gpd.GeoSeries([dallas_buffer], crs=counties_projected.crs).plot(ax=ax, color="orange", alpha=0.4)
gpd.GeoSeries([dallas_geometry], crs=counties_projected.crs).plot(ax=ax, color="red", alpha=0.7);
```

## Discussion

What might a buffer represent in a real project?

Examples:

- distance from a school
- distance from a grocery store
- distance from a transit stop
- distance from a shelter
- distance from a park

# 7. Spatial Relationships

Spatial data lets us ask questions that ordinary tables cannot answer
directly.

For example:

> Which counties touch Dallas County?


```python
counties_projected["touches_dallas"] = counties_projected.touches(dallas_geometry)
```


```python
counties_projected[counties_projected["touches_dallas"]][["COUNTY", "touches_dallas"]]
```


```python
counties_projected.plot(column="touches_dallas", categorical=True, legend=True, figsize=(10, 10));
```

We can also ask which counties intersect our Dallas buffer.


```python
counties_projected["intersects_dallas_buffer"] = counties_projected.intersects(dallas_buffer)
```


```python
counties_projected[counties_projected["intersects_dallas_buffer"]][["COUNTY", "intersects_dallas_buffer"]].head(20)
```


```python
counties_projected.plot(column="intersects_dallas_buffer", categorical=True, legend=True, figsize=(10, 10));
```

# 8. Merging Census Data with Geography

Now we will connect this mapping work to the ACS dataset from the
previous session.


```python
acs = pd.read_csv("data/texas_ACS_2024_sample.csv")
```


```python
acs.head()
```

The ACS file has long column names. Let’s clean them up.


```python
acs = acs.rename(columns={
    "name": "county",
    "Total Population": "population",
    "Population 16 years and over in labor force": "labor_force",
    "Population 25 years and over with Bachelors Degree or higher": "bachelors_degree",
    "Population 16 and over with income in the past 12 months below poverty level": "poverty_population",
    "Households with Internet Subscription": "internet_households",
    "Total Population in Renter Occupied Housing": "renters",
    "Median Household Income in the Past 12 Months (In 2024 Inflation-adjusted Dollars)": "median_income",
    "Total Population in Owner Occupied Housing": "owners"
})
```

The GeoJSON file uses a five-digit county FIPS code. The ACS file stores
that FIPS code at the end of the `geoid` field.


```python
acs["FIPS"] = acs["geoid"].str[-5:]
```

Let’s create a few rates that are easier to compare across counties than
raw totals.


```python
acs["poverty_rate"] = acs["poverty_population"] / acs["labor_force"]
acs["bachelors_rate"] = acs["bachelors_degree"] / acs["population"]
acs["internet_rate"] = acs["internet_households"] / (acs["owners"] + acs["renters"])
acs["renter_rate"] = acs["renters"] / (acs["owners"] + acs["renters"])
```


```python
acs[["county", "FIPS", "population", "median_income", "poverty_rate", "internet_rate"]].head()
```

Now we can merge the ACS data onto the county shapes.


```python
mapped = counties_projected.merge(acs, on="FIPS", how="left")
```


```python
mapped.head()
```

Check whether the merge worked.


```python
mapped[["COUNTY", "county", "population", "median_income"]].head()
```

# 9. Choropleth Maps

A choropleth map colors areas based on a data value.

Let’s map median household income.


```python
mapped.plot(
    column="median_income",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2
);
```

This map is more informative if we remove the axes.


```python
ax = mapped.plot(
    column="median_income",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2
)
ax.set_axis_off();
```

## You Try

Map `poverty_rate` instead of `median_income`.


```python

# Your code here

```

# 10. Totals vs Rates

Raw totals often mostly show where the most people live.

Rates can reveal different kinds of patterns.

Compare total population with internet access rate.


```python
ax = mapped.plot(
    column="population",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2
)
ax.set_axis_off();
```


```python
ax = mapped.plot(
    column="internet_rate",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2
)
ax.set_axis_off();
```

## Discussion

What does the population map show clearly?

What does the internet access map show that the population map does not?

# 11. Classifying Values

Sometimes maps are easier to read if values are grouped into categories.

One common option is to use quantiles, which divide counties into groups
with similar numbers of counties in each group.


```python
ax = mapped.plot(
    column="median_income",
    scheme="quantiles",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2
)
ax.set_axis_off();
```

## Discussion

How does this map differ from the previous income map?

Which version is easier to interpret?

# 12. Adding the Parks Dataset

Now we will add a second dataset: Texas state parks by county.

This dataset gives us a good example of a variable that is interesting,
geographic, and not obviously connected to the ACS variables.


```python
parks = pd.read_csv("data/texas_state_parks_by_county.csv")
```

If your file has a slightly different name, update the path above.


```python
parks.head()
```

Clean the parks data so the FIPS code matches our other datasets.


```python
parks["FIPS"] = parks["geoid"].str[-5:]
parks = parks.rename(columns={"state_park_count": "parks"})
```


```python
parks.head()
```

Merge parks onto our mapped data.


```python
mapped = mapped.merge(parks[["FIPS", "parks"]], on="FIPS", how="left")
```


```python
mapped[["COUNTY", "parks"]].head(10)
```

Some counties do not have state parks in this dataset.

That gives us a chance to talk about missing data.


```python
mapped["parks"].isna().sum()
```

# 13. Mapping Missing Data

GeoPandas ignores missing values by default.


```python
ax = mapped.plot(
    column="parks",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2
)
ax.set_axis_off();
```

We can make the missing data visible using `missing_kwds`.


```python
ax = mapped.plot(
    column="parks",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2,
    missing_kwds={
        "color": "lightgrey",
        "edgecolor": "red",
        "hatch": "///",
        "label": "No state park listed"
    }
)
ax.set_axis_off();
```

Missing data is not always a problem to hide. Sometimes it is part of
the story.

# 14. Filling Missing Values

In this case, a missing value probably means the county has zero state
parks in the parks dataset.

Let’s create a filled version.


```python
mapped["parks_filled"] = mapped["parks"].fillna(0)
```


```python
ax = mapped.plot(
    column="parks_filled",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2
)
ax.set_axis_off();
```

## Discussion

Is it reasonable to replace missing park values with zero?

When would filling missing values be dangerous?

# 15. Comparing Maps

Now we can compare the parks map to ACS variables.

For example:

- Are parks concentrated in high-income counties?
- Are parks concentrated in rural counties?
- Are parks related to population?
- Are parks just showing geography and land availability?


```python
mapped.plot.scatter(x="parks_filled", y="median_income");
```


```python
mapped.plot.scatter(x="parks_filled", y="population");
```


```python
mapped.plot.scatter(x="parks_filled", y="poverty_rate");
```

## Discussion

Do these relationships look strong, weak, or messy?

What might explain the pattern?

What might be misleading?

# 16. Layering Maps

We can layer different geometries on the same map.

For example, we can map county income and overlay county centroids.


```python
ax = mapped.plot(
    column="median_income",
    legend=True,
    figsize=(10, 10),
    edgecolor="black",
    linewidth=0.2
)
mapped.set_geometry("centroid").plot(ax=ax, color="black", markersize=3)
ax.set_axis_off();
```

This is a simple example, but layering is a very important idea in
spatial analysis.

# 17. Pulling it all together: Parks Within 100 km of a County

Now let's tie several spatial ideas together.

Suppose we want to ask:

> How many state parks could someone plausibly drive to within 100 km of a given county?

This example uses:
- projections
- centroids
- buffers
- spatial relationships
- missing data
- counting and summarizing

### Important Limitation

Our parks dataset counts parks by county, but it does not include the exact latitude and longitude of each park.

So this is an approximation.

We will estimate nearby parks by counting parks in counties whose centroids fall within 100 km of the selected county.

A better version of this analysis would use the actual point locations of each state park.


```python


```


```python
# Choose a county
selected_county = "Dallas County"

target_county = counties_projected[
    counties_projected["COUNTY"] == selected_county
]

target_county
```


```python
# Create a 100 km buffer around that county
buffer_100km = target_county.geometry.buffer(100000).iloc[0]
```


```python
# Merge park counts onto our projected county data
counties_with_parks = counties_projected.merge(
    parks,
    left_on="FIPS",
    right_on="FIPS",
    how="left"
)
```


```python
# Counties with no matching park record should have zero parks
counties_with_parks["parks"] = (
    counties_with_parks["parks"].fillna(0)
)
```


```python
# Identify counties whose centroids fall within the 100 km buffer
counties_with_parks["within_100km"] = (
    counties_with_parks["centroid"].within(buffer_100km)
)
```


```python
# Count the estimated number of nearby parks
nearby_parks = counties_with_parks[
    counties_with_parks["within_100km"]
]["parks"].sum()

print(nearby_parks)
```


```python
# Show the counties included in the estimate
counties_with_parks[
    counties_with_parks["within_100km"]
][["COUNTY", "parks"]].sort_values(
    by="parks",
    ascending=False
)
```


```python
# Plot the result
ax = counties_projected.plot(
    color="white",
    edgecolor="lightgray",
    figsize=(10, 10)
)

# Highlight the selected county
target_county.plot(
    ax=ax,
    color="orange",
    edgecolor="black"
)

# Draw the 100 km buffer
gpd.GeoSeries([buffer_100km], crs=counties_projected.crs).plot(
    ax=ax,
    color="none",
    edgecolor="red",
    linewidth=2
)

# Highlight counties whose centroids are within the buffer
counties_with_parks[counties_with_parks["within_100km"]].plot(
    ax=ax,
    color="lightblue",
    edgecolor="black",
    alpha=0.5
)

# Plot county centroids, scaled by park count
counties_with_parks.set_geometry("centroid").plot(
    ax=ax,
    markersize=counties_with_parks["parks"] * 10,
    color="green",
    alpha=0.7
)

ax.set_title(
    f"Estimated state parks within 100 km of {selected_county} County: {nearby_parks}"
)

ax.set_axis_off()
```

### Discussion

What assumptions did we make?

- We used county centroids instead of exact park locations.
- We counted all parks in a county if that county's centroid was within the buffer.
- We used a straight-line distance, not actual driving distance.
- We did not consider roads, traffic, park size, or park quality.

This is still useful because it shows how spatial analysis can help us build an approximate answer when perfect data is not available.

# 18. Mini-Challenge

Create one map that surprises you.

Choose one variable from the dataset and make a map.

Possible variables:

- `population`
- `median_income`
- `poverty_rate`
- `bachelors_rate`
- `internet_rate`
- `renter_rate`
- `parks_filled`
- `SQUARE_MIL`
- `distance_to_dallas`

Your map should include:

1.  a variable
2.  a legend
3.  no axes
4.  a short interpretation


```python

# Your map here

```

What pattern do you see?

Why might this pattern exist?

What might be misleading about this map?

# AI Assistant Checkpoint

Try asking an AI assistant one of the following:

- Explain what a geometry column is in GeoPandas.
- Explain the difference between a choropleth map and a scatterplot.
- Why can raw totals be misleading on maps?
- What does it mean for two polygons to touch or intersect?
- Help me improve this GeoPandas map without changing the data.

AI tools are most useful when you ask them to explain, debug, or
critique your work.

# Wrap-Up

Today we learned that spatial data is more than just a picture.

We used GeoPandas to:

- load county boundaries
- map geometries
- calculate areas, centroids, distances, and buffers
- test spatial relationships
- merge ACS data with county shapes
- map missing values
- compare geographic patterns

The most important idea is:

> Geography changes how we interpret data.
