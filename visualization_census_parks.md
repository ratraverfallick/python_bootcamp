# Python, Data Visualization, and Census Data

 -- Dr. Eric Godat

## Session Goals

Today we will use Python to move from **data** to **questions** to
**evidence**.

By the end of this notebook, you should be able to:

- load and inspect tabular data
- create new columns from existing columns
- filter, sort, and summarize data
- make basic visualizations with pandas and matplotlib
- combine two datasets using a shared key
- begin asking questions with Census/ACS data

The big idea today is:

> Data science is not just about running code. It is about deciding what
> matters, testing assumptions, and interpreting patterns.


```python
import pandas as pd
import matplotlib.pyplot as plt

%matplotlib inline
%config InlineBackend.figure_format = 'retina'
```

# Part 1: Finding the Best Apartment

Suppose you are trying to find the best apartment near campus.

Different apartments have different:

- rent
- size
- number of bedrooms
- number of bathrooms
- distance to campus
- rating
- pet rules

Before we write any code, discuss:

> What makes an apartment “good”?

There is no single correct answer. That is what makes this a useful data
science problem.


```python
apartments = {
    "Apartment": [
        "Mockingbird Flats",
        "Highland House",
        "Campus View",
        "The Village",
        "Oak Lawn Lofts",
        "Greenville Commons",
        "Mustang Station",
        "The Hidden Gem"
    ],
    "Rent": [950, 1450, 1100, 1700, 2100, 1250, 800, 1350],
    "Bedrooms": [1, 2, 1, 3, 2, 2, 1, 2],
    "Bathrooms": [1, 2, 1, 2, 2, 1, 1, 2],
    "Sqft": [550, 980, 620, 1400, 1150, 870, 450, 760],
    "Distance": [0.4, 1.8, 0.2, 3.5, 2.2, 1.1, 0.1, 0.9],
    "Rating": [3.8, 4.6, 3.2, 4.9, 4.7, 4.1, 2.9, 4.8],
    "pets_allowed": [
        "cat only",
        "dog and cat",
        "no",
        "all pets",
        "dog and cat",
        "cat only",
        "no",
        "all pets"
    ]
}

apts = pd.DataFrame(apartments)
apts
```

## First Impressions

Before calculating anything:

> Which apartment looks best to you? Why?

This is an important first step. Humans often have strong intuitions
about data before doing formal analysis.

## Question 1: Which apartment is cheapest?


```python
apts.sort_values(by="Rent")
```

## Question 2: Which apartment gives the most space per dollar?


```python
apts["sqft_per_dollar"] = apts["Sqft"] / apts["Rent"]
apts[["Apartment", "Rent", "Sqft", "sqft_per_dollar"]].sort_values(
    by="sqft_per_dollar",
    ascending=False
)
```

## Question 3: Which apartments meet a student’s basic needs?

Suppose a student wants:

- at least 2 bedrooms
- rent below \$1500


```python
apts[(apts["Bedrooms"] >= 2) & (apts["Rent"] < 1500)]
```

## Building a Bad Model First

Let’s try to build a simple apartment score.

This first model rewards:

- more space per dollar
- higher rating


```python
apts["simple_score"] = apts["sqft_per_dollar"] * apts["Rating"]

apts[["Apartment", "Rent", "Sqft", "Distance", "Rating", "pets_allowed", "simple_score"]].sort_values(
    by="simple_score",
    ascending=False
)
```

## Discussion

Does this ranking feel right?

What is missing from the model?

Some possibilities:

- distance to campus
- pet policy
- parking
- safety
- noise
- roommates
- public transit
- laundry
- whether rent includes utilities

The model reflects the variables we choose to include.

## Improving the Model

Distance matters. Smaller distances should get better scores.

One simple way to reward closer apartments is to use:


1 / distance


```python
apts["distance_score"] = 1 / apts["Distance"]

apts[["Apartment", "Distance", "distance_score"]].sort_values(
    by="distance_score",
    ascending=False
)
```

Now combine value, rating, and distance.


```python
apts["final_score"] = (
    apts["sqft_per_dollar"]
    * apts["Rating"]
    * apts["distance_score"]
)
```


```python
apts[["Apartment", "Rent", "Sqft", "Distance", "Rating", "pets_allowed", "final_score"]].sort_values(
    by="final_score",
    ascending=False
)
```

## You Try

Modify the score.

Ideas:

- include number of bedrooms
- make rent matter more
- make distance matter less
- create a filter for apartments that allow pets
- remove apartments below a certain rating


```python

# Your code here

```

## AI Assistant Checkpoint

Ask an AI assistant one of the following:

- “Explain how this apartment score works in plain English.”
- “What assumptions are built into this formula?”
- “How could I change the formula to care more about distance?”

Then decide whether the suggestion actually makes sense.

# Part 2: Visualizing the Apartment Data

A good visualization starts with a question.

Not:

> What plot can I make?

But:

> What question am I trying to answer?

## Question: How much does each apartment cost?


```python
apts.plot.bar(x="Apartment", y="Rent", figsize=(10, 5), legend=False)
plt.ylabel("Monthly Rent")
plt.title("Apartment Rent Comparison")
plt.xticks(rotation=45, ha="right");
```

## Question: Are more expensive apartments rated better?


```python
apts.plot.scatter(x="Rent", y="Rating", figsize=(7, 5))
plt.title("Rent vs. Rating")
plt.xlabel("Monthly Rent")
plt.ylabel("Rating");
```

## Question: How are final scores distributed?


```python
apts["final_score"].plot.hist(figsize=(7, 5))
plt.title("Distribution of Apartment Scores")
plt.xlabel("Final Score");
```

## Question: Which apartment wins under our model?


```python
apts.sort_values(by="final_score", ascending=False).plot.bar(
    x="Apartment",
    y="final_score",
    figsize=(10, 5),
    legend=False
)
plt.ylabel("Final Score")
plt.title("Apartment Ranking from Our Model")
plt.xticks(rotation=45, ha="right");
```

## Visualization Reflection

For each plot, ask:

- What question does this plot answer?
- What does this plot hide?
- What choices did we make when creating it?
- Could a different plot tell a different story?

# Part 3: Moving to Public Data — Texas ACS/Census Sample

Now we will use the same basic pandas and plotting skills on real public
data.

This dataset is a prepared sample from the American Community Survey for
Texas counties.

We are using a prepared CSV so that we can focus on analysis rather than
API syntax.


```python
acs = pd.read_csv("data/texas_ACS_2024_sample.csv")
acs.head()
```

## Inspect the Data


```python
acs.shape
```


```python
acs.columns
```


```python
acs.head(3)
```

## Clean Up Column Names

Real datasets often have long or awkward column names. It is common to
rename columns before analysis.


```python
acs = acs.rename(columns={
    "geoid": "geoid",
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

acs = acs.drop(columns=["Unnamed: 0"], errors="ignore")
acs.head()
```

## Create a Short County Name

The county names currently look like `Dallas County, TX`. For some joins
and labels, it is useful to make a shorter county name.


```python
acs["county_name"] = acs["county"].str.replace(" County, TX", "", regex=False)
acs[["county", "county_name"]].head()
```

# Part 4: Beyond Totals

A common beginner mistake is to compare raw totals without accounting
for population size.

Big counties often dominate totals simply because they have more people.

## Which counties have the largest population?


```python
acs[["county", "population"]].sort_values(by="population", ascending=False).head(10)
```

## Which counties have the most people with bachelor’s degrees?


```python
acs[["county", "population", "bachelors_degree"]].sort_values(
    by="bachelors_degree",
    ascending=False
).head(10)
```

Is this surprising?

Or are we mostly seeing the biggest counties again?

## Create Rates and Percentages

Rates often make comparisons more meaningful.


```python
acs["bachelors_rate"] = acs["bachelors_degree"] / acs["population"]
acs["poverty_rate"] = acs["poverty_population"] / acs["labor_force"]
acs["renter_rate"] = acs["renters"] / (acs["renters"] + acs["owners"])
acs["internet_rate"] = acs["internet_households"] / (acs["renters"] + acs["owners"])
```


```python
acs[[
    "county",
    "population",
    "median_income",
    "bachelors_rate",
    "poverty_rate",
    "renter_rate",
    "internet_rate"
]].head()
```

## Which counties have the highest bachelor’s degree rate?


```python
acs[["county", "population", "bachelors_rate"]].sort_values(
    by="bachelors_rate",
    ascending=False
).head(10)
```

## Which counties have the highest poverty rate?


```python
acs[["county", "population", "poverty_rate"]].sort_values(
    by="poverty_rate",
    ascending=False
).head(10)
```

## Discussion

How did the rankings change when we moved from totals to rates?

Why does that matter?

# Part 5: Visualizing ACS Relationships

## Question: Is education related to income?


```python
acs.plot.scatter(
    x="bachelors_rate",
    y="median_income",
    figsize=(7, 5)
)
plt.title("Bachelor's Degree Rate vs. Median Income")
plt.xlabel("Bachelor's Degree Rate")
plt.ylabel("Median Household Income");
```

## Question: Is poverty related to internet access?


```python
acs.plot.scatter(
    x="poverty_rate",
    y="internet_rate",
    figsize=(7, 5)
)
plt.title("Poverty Rate vs. Internet Access Rate")
plt.xlabel("Poverty Rate")
plt.ylabel("Internet Access Rate");
```

## Question: Are renter-heavy counties different?


```python
acs.plot.scatter(
    x="renter_rate",
    y="median_income",
    figsize=(7, 5)
)
plt.title("Renter Rate vs. Median Income")
plt.xlabel("Renter Rate")
plt.ylabel("Median Household Income");
```

## You Try

Choose two variables and make a scatterplot.

Possible columns:

- population
- median_income
- bachelors_rate
- poverty_rate
- renter_rate
- internet_rate


```python

# Your code here

```

## Finding Outliers

Scatterplots help us see patterns, but sometimes we want to inspect
specific rows.

Let’s look at counties with high income but lower bachelor’s degree
rates.


```python
acs[(acs["median_income"] > 80000) & (acs["bachelors_rate"] < 0.20)][[
    "county", "population", "median_income", "bachelors_rate"
]].sort_values(by="median_income", ascending=False)
```

## Discussion

What might explain outliers?

Possible answers:

- industry mix
- rural/urban differences
- oil and gas economies
- military bases
- small population effects
- measurement issues

This is where data analysis becomes interpretation.

# Part 6: Adding a Dataset — Texas State Parks by County

Now we will bring in a second dataset.

This is useful because real projects often require combining information
from multiple sources.


```python
parks = pd.read_csv("data/texas_state_parks_by_county.csv")
parks.head()
```

## Inspect the Parks Data


```python
parks.shape
```


```python
parks.columns
```

## Merge ACS and Parks Data

Both datasets include a `geoid` column. That gives us a shared key for
merging.


```python
merged = pd.merge(
    acs,
    parks[["geoid", "state_park_count"]],
    on="geoid",
    how="left"
)
```


```python
merged[["county", "population", "median_income", "state_park_count"]].head()
```

Some counties do not have a state park. Those counties will show up with
missing values.

For this exercise, we will replace missing park counts with 0.


```python
merged["state_park_count"] = merged["state_park_count"].fillna(0)
```


```python
merged[["county", "population", "median_income", "state_park_count"]].sort_values(
    by="state_park_count",
    ascending=False
).head(15)
```

## Create Parks Per Person

Raw park counts can be misleading. A county with 1 park and 5,000 people
is different from a county with 1 park and 5 million people.


```python
merged["parks_per_100k"] = merged["state_park_count"] / merged["population"] * 100000
```


```python
merged[["county", "population", "state_park_count", "parks_per_100k"]].sort_values(
    by="parks_per_100k",
    ascending=False
).head(15)
```

## Question: Do counties with more parks have higher income?


```python
merged.plot.scatter(
    x="state_park_count",
    y="median_income",
    figsize=(7, 5)
)
plt.title("State Park Count vs. Median Income")
plt.xlabel("Number of State Parks")
plt.ylabel("Median Household Income");
```

## Question: Do counties with more parks per person have different poverty rates?


```python
merged.plot.scatter(
    x="parks_per_100k",
    y="poverty_rate",
    figsize=(7, 5)
)
plt.title("State Parks Per 100k People vs. Poverty Rate")
plt.xlabel("State Parks per 100,000 People")
plt.ylabel("Poverty Rate");
```

## Discussion

State parks are a useful curveball variable.

They might relate to:

- geography
- tourism
- rural land use
- public investment
- quality of life
- population density

But they might not directly explain income, poverty, or internet access.

This is a good reminder:

> Interesting correlations are not automatically meaningful
> explanations.

# Part 7: Final Mini-Challenge

Choose one question to investigate.

## Option A: Apartment Model

Modify the apartment score and explain why your version is better.

## Option B: ACS Exploration

Find two ACS variables that seem related. Make a plot and describe the
pattern.

## Option C: Parks Curveball

Explore whether state parks appear related to income, poverty, internet
access, or population.

## Option D: Outlier Hunt

Find one county that surprises you and explain why.


```python

# Your code here

```

# AI Assistant Checkpoint

Ask an AI assistant one of the following:

- “What are three possible explanations for this pattern?”
- “What might be misleading about this plot?”
- “How could I normalize this variable?”
- “What other data would I need to answer this question better?”

Then evaluate the answer. AI can help generate ideas, but you are
responsible for deciding whether those ideas make sense.

# Wrap-Up

Today we practiced:

- loading data
- creating new columns
- filtering and sorting
- building simple scoring models
- making plots
- comparing totals and rates
- merging datasets
- interpreting patterns carefully

The most important lesson is:

> Data science is not just computation. It is interpretation.

In the next session, we will extend these ideas by adding geography and
mapping.
