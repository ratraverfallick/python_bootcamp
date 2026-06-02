# Introduction to Python and Jupyter

 -- Dr. Eric Godat

## Session Goal

In this session, we are not trying to become professional software
engineers.

Instead, we will use Python as a tool for exploring, analyzing, and
visualizing data.

By the end of this notebook, you will be able to:

- run Python code in a Jupyter notebook
- use variables, lists, dictionaries, loops, and functions at a basic
  level
- create and inspect pandas DataFrames
- select, filter, group, and merge tabular data
- make a simple plot
- use AI tools productively when you get stuck

# Operating This Notebook

This document is a Jupyter notebook. Notebooks let us combine text,
code, and output in one place.

A notebook is made of cells. Some cells contain text, like this one.
Other cells contain code.

To run a code cell:

- click inside the cell
- press **Shift + Enter**

The most important thing to remember is that notebooks run code one cell
at a time.

If something breaks:

1.  Restart the kernel
2.  Run the cells again from the top

This fixes many common notebook problems.

# Basic Math

Python can do simple mathematical operations like a calculator.


```python
1 + 1
```


```python
2.5 - 2.0
```


```python
10 * 4
```


```python
25 / 5
```

# Variables

Variables let us store information so we can use it later.

A variable name goes on the left. The value goes on the right.


```python
Dallas_population = 1300000
SMU_students = 12000
```


```python
Dallas_population + SMU_students
```

When we assign a value to a variable, Python stores it, but it does not
automatically show the value.


```python
total_people = Dallas_population + SMU_students
```

To display the value, we can call the variable name.


```python
total_people
```

We can also use the `print()` function.


```python
print(total_people)
```

# Strings

Strings are used for text.

In Python, strings are surrounded by quotation marks.


```python
course_name = "Introduction to Python and Data Science"
course_name
```


```python
print(course_name)
```

# Lists

Lists let us store multiple values together.

Lists are enclosed by square brackets and separated by commas.


```python
cities = ["Dallas", "Houston", "Austin", "San Antonio"]
cities
```

We can access individual elements in a list using their position.

Python starts counting at 0.


```python
cities[0]

```


```python
cities[1]

```

We can also access the last item with `-1`.


```python
cities[-1]

```

The `len()` function tells us how many items are in a list.


```python
len(cities)

```

## You Try

Create a list containing:

- your hometown
- your favorite movie
- your favorite food

Then print the first and last item in your list.


```python

# Your code here

```

# Dictionaries

Dictionaries store information using keys and values.

They are useful when we want to look something up by name.


```python
city_populations = {
    "Dallas": 1300000,
    "Taos": 5668,
    "Houston": 2300000
}

```


```python
city_populations["Taos"]

```

Dictionaries are especially useful when we want to label or translate
values in a dataset.


# Libraries

Python libraries are collections of pre-written tools.

Instead of writing everything from scratch, we can import libraries that
help us:

- analyze data
- make plots
- work with maps
- clean text

One of the most important libraries for data science is **pandas**.


```python
import pandas as pd

```

The phrase `as pd` gives pandas a shorter nickname. This is a common
convention.

# DataFrames and Pandas

A pandas DataFrame is like a spreadsheet inside Python.

It has rows and columns, but unlike a spreadsheet, every step can be
saved, repeated, and shared.

Let’s create a small DataFrame about Tolkien books and word counts.


```python
d = {
    "Books": [
        "The Silmarillion",
        "The Hobbit",
        "The Fellowship of the Ring",
        "The Two Towers",
        "The Return of the King"
    ],
    "Words": [130115, 95506, 187726, 156147, 137037]
}

df = pd.DataFrame(d)
df

```

# Inspecting a DataFrame

The first thing we usually do with a dataset is inspect it.


```python
df.head()

```


```python
df.info()

```


```python
df.describe()

```


# Selecting Columns and Rows

We can select a single column by using its name.


```python
df["Books"]

```


```python
df["Words"]
```

We can select rows using slices.


```python
df[:2]
```


```python
df[2:4]

```

We can also combine column selection and row selection.


```python
df["Words"][2:4]

```

## You Try

1.  Select only the `Words` column
2.  Select the first 3 rows
3.  Find the total number of words across all books


```python

# Your code here

```

# Doing Calculations With Columns

Because the `Words` column contains numbers, we can calculate with it.


```python
total_words = df["Words"].sum()
total_words

```


```python
average_words = df["Words"].mean()
average_words

```


```python
max_words = df["Words"].max()
max_words

```

# Filtering Data

Filtering lets us select rows that match a condition.

First, let’s ask a True/False question:


```python
df["Words"] > 150000

```

Now we can use that True/False result to filter the DataFrame.


```python
df[df["Words"] > 150000]
```

We can also filter text.


```python
df[df["Books"].str.contains("ing")]
```

We can combine conditions using `&` for AND.

Each condition must be inside parentheses.


```python
df[(df["Books"].str.contains("ing")) & (df["Words"] > 150000)]
```

## You Try

Filter the DataFrame to show only books with fewer than 140,000 words.


```python

# Your code here

```

# Loops

Loops let us repeat an action.

For data science, loops are useful when we want to do something once for
each item in a collection.


```python
for book in df["Books"]:
    print(book)

```

Notice the indentation. The indented line is the part that repeats.


# Functions

Functions let us package a small set of instructions so we can reuse
them.


```python
def square_me(n):
    return n * n
```


```python
square_me(4)
```

For this bootcamp, you do not need to memorize how to write complex
functions.

The most important idea is that functions take an input, do something,
and often return an output.


# More Complex Data

Now let’s work with a more detailed dataset.

Below we have chapters from *The Hobbit* and *The Lord of the Rings*,
along with word counts.

Each chapter is represented as a list. All of those chapter lists are
stored inside one larger list.


```python
chapters = [[0,1,'An Unexpected Party',8638,0],
            [0,2,'Roast Mutton',5257,0],
            [0,3,'A Short Rest',2876,0],
            [0,4,'Over Hill and Under Hill',4034,0],
            [0,5,'Riddles in the Dark',6967,0],
            [0,6,'Out of the Frying Pan into the Fire',6703,0],
            [0,7,'Queer Lodgings',9027,0],
            [0,8,'Flies and Spiders',10223,0],
            [0,9,'Barrels Out of Bond',5833,0],
            [0,10,'A Warm Welcome',3930,0],
            [0,11,'On the Doorstep',3001,0],
            [0,12,'Inside Information',7132,0],
            [0,13,'Not At Home',3909,0],
            [0,14,'Fire and Water',3236,0],
            [0,15,'The Gathering of the Clouds',3362,0],
            [0,16,'A Thief in the Night',2153,0],
            [0,17,'The Clouds Burst',3949,0],
            [0,18,'The Return Journey',2815,0],
            [0,19,'The Last Stage',2461,0],
            [1,-4,'Concerning Hobbits',3406,1],
            [1,-3,'Concerning Pipeweed',600,1],
            [1,-2,'Of the Ordering of the Shire',2431,1],
            [1,-1,'Note on the Shire Records',914,1],
            [1,1,'A Long-expected Party',10012,1],
            [1,2,'The Shadow of the Past',11311,1],
            [1,3,'Three is Company',9763,1],
            [1,4,'A Short Cut to Mushrooms',5957,1],
            [1,5,'A Conspiracy Unmasked',5196,1],
            [1,6,'The Old Forest',6502,1],
            [1,7,'In the House of Tom Bombadil',5501,1],
            [1,8,'Fog on the Barrow-downs',6694,1],
            [1,9,'At the Sign of the Prancing Pony',6251,1],
            [1,10,'Strider',5905,1],
            [1,11,'A Knife in the Dark',9468,1],
            [1,12,'Flight to the Ford',8805,1],
            [1,1,'Many Meetings',9085,2],
            [1,2,'The Council of Elrond',16360,2],
            [1,3,'The Ring goes South',10656,2],
            [1,4,'A Journey in the Dark',11501,2],
            [1,5,'The Bridge of Khazad-dum',5428,2],
            [1,6,'Lothlorien',9387,2],
            [1,7,'The Mirror of Galadriel',6896,2],
            [1,8,'Farewell to Lorien',6174,2],
            [1,9,'The Great River',7218,2],
            [1,10,'The Breaking of the Fellowship',6305,2],
            [2,1,'The Departure of Boromir',3397,3],
            [2,2,'The Riders of Rohan',11133,3],
            [2,3,'The Uruk-hai',7854,3],
            [2,4,'Treebeard',12876,3],
            [2,5,'The White Rider',8856,3],
            [2,6,'The King of the Golden Hall',9303,3],
            [2,7,"Helm's Deep",7575,3],
            [2,8,'The Road to Isengard',7899,3],
            [2,9,'Flotsam and Jetsam',7789,3],
            [2,10,'The Voice of Saruman',5663,3],
            [2,11,'The Palantir',6325,3],
            [2,1,'The Taming of Smeagol',8375,4],
            [2,2,'The Passage of the Marshes',7357,4],
            [2,3,'The Black Gate is Closed',5881,4],
            [2,4,'Of Herbs and Stewed Rabbit',6975,4],
            [2,5,'The Window on the West',10120,4],
            [2,6,'The Forbidden Pool',5179,4],
            [2,7,'Journey to the Crossroads',4266,4],
            [2,8,'The Stairs of Cirith Ungol',6793,4],
            [2,9,"Shelob's Lair",5209,4],
            [2,10,'The Choices of Master Samwise',7322,4],
            [3,1,'Minas Tirith',13100,5],
            [3,2,'The Passing of the Grey Company',8586,5],
            [3,3,'The Muster of Rohan',6951,5],
            [3,4,'The Siege of Gondor',11793,5],
            [3,5,'The Ride of the Rohirrim',4358,5],
            [3,6,'The Battle of the Pelennor Fields',5225,5],
            [3,7,'The Pyre of Denethor',3736,5],
            [3,8,'The Houses of Healing',6731,5],
            [3,9,'The Last Debate',5416,5],
            [3,10,'The Black Gate Opens',5204,5],
            [3,1,'The Tower of Cirith Ungol',9721,6],
            [3,2,'The Land of Shadow',8446,6],
            [3,3,'Mount Doom',7777,6],
            [3,4,'The Field of Cormallen',4721,6],
            [3,5,'The Steward and the King',7639,6],
            [3,6,'Many Partings',7440,6],
            [3,7,'Homeward Bound',4106,6],
            [3,8,'The Scouring of the Shire',11296,6],
            [3,9,'The Grey Havens',4791,6]
           ]
```

Now we can turn the list of lists into a DataFrame.


```python
cols = ['CollectionNum', 'ChapterNum', 'ChapterName', 'WordCount', 'BookNum']
data = pd.DataFrame(chapters, columns=cols)
data

```

------------------------------------------------------------------------

# Adding Labels With map()

The `CollectionNum` column uses numbers to represent each collection.

We can make the data easier to read by mapping those numbers to names.


```python
title_map = {
    0: 'The Hobbit',
    1: 'The Fellowship of the Ring',
    2: 'The Two Towers',
    3: 'The Return of the King'
}
```


```python
data['CollectionName'] = data['CollectionNum'].map(title_map)
data.head()

```

# GroupBy

GroupBy allows us to summarize data.

This is one of the most common operations in data science.

For example, we can count how many rows belong to each collection.


```python
data.groupby('CollectionName').count()

```

We can also calculate the total word count for each collection.


```python
data.groupby('CollectionName')['WordCount'].sum()

```

## You Try

Use `groupby` to find the average chapter length for each collection.


```python

# Your code here

```

# Joining DataFrames

Merging allows us to combine information from multiple datasets.

Here is a second DataFrame with publication years.


```python
dates = pd.DataFrame({
    'Books': df['Books'],
    'Year': [1977, 1937, 1954, 1954, 1955]
})

dates
```

Now we can merge the word count DataFrame with the publication year
DataFrame.


```python
merged = pd.merge(df, dates, on='Books', how='inner')
merged

```

------------------------------------------------------------------------

# Plotting

Visualizing data helps us understand and communicate patterns.

Pandas has simple plotting tools built in.


```python

import matplotlib.pyplot as plt

```


```python

plot = df.plot(kind='bar', x='Books', y='Words', legend=False)
plot.set_ylabel('Word Count')
plot.set_title('Word Counts for Tolkien Books')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()

```

## You Try

Try changing the plot type from `bar` to another option, such as:

- `line`
- `barh`


```python

# Your code here

```

# AI Assistant Checkpoint

AI tools can be helpful when you get stuck, but they are most useful
when you ask them to explain or debug code.

Try asking an AI assistant one of the following:

- “Explain what this dataframe operation is doing.”
- “Why does this filtering operation work?”
- “How would I change this plot color?”
- “What does groupby do in pandas?”

The goal is not just to get an answer. The goal is to understand what
the code is doing.

------------------------------------------------------------------------

# Mini Challenge

Using the Tolkien DataFrames from this notebook:

1.  Find the longest book
2.  Filter books with more than 150,000 words
3.  Create a plot of word counts
4.  Use `groupby` to find the total word count for each collection
5.  Ask an AI assistant to explain one line of code you found confusing


```python

# Your code here

```

# Wrapping Up

In this session, we covered:

- Jupyter notebook basics
- basic Python math
- variables
- strings
- lists
- dictionaries
- loops
- functions
- pandas DataFrames
- selecting and filtering data
- grouping data
- merging DataFrames
- basic plotting
- using AI tools to support learning

The most important takeaway is this:

> Python is a tool for asking questions about data.

You do not need to memorize every command. You need to practice reading
code, modifying it, running it, and interpreting the results.
