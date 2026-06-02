# Working with Text and Text Analytics

 -- Dr. Eric Godat

So far in this bootcamp we have worked with:

- numbers
- tables
- maps
- geographic data

But a huge amount of real-world data is actually text.

Reviews, news articles, social media posts, interview transcripts,
emails, books, and surveys are all forms of text data.

Today we will explore:

- cleaning text
- counting words and phrases
- extracting patterns from language
- identifying important words in documents
- some limitations of text analysis

## Imports


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from collections import Counter
import string

%matplotlib inline
%config InlineBackend.figure_format = "retina"
```

# Apartment Reviews — Text as Data

Let’s start with a small dataset of apartment reviews.

Unlike numerical data, text is:

- messy
- inconsistent
- emotional
- ambiguous

But we can still analyze it computationally.


```python
reviews = pd.DataFrame({
    "Apartment":[
        "Mustang Station",
        "Campus View",
        "Highland House",
        "Mockingbird Flats",
        "Hilltop Lofts",
        "University Crossing",
        "Maple Terrace",
        "Lakeside Apartments"
    ],

    "Stars":[
        2,4,1,5,3,2,4,1
    ],

    "Review":[
        "The location is honestly great if you need to get to campus quickly, but the walls are paper thin and people are constantly yelling outside at like 2am.",

        "Pretty solid place overall. Cheap rent, decent kitchen, and I can walk to class in about 10 minutes. Maintenance is slow sometimes though.",

        "I would absolutely not live here again. My AC broke twice during August and management stopped answering emails after the first week.",

        "Loved living here. The staff remembered my name, the study rooms were actually useful, and the coffee shop downstairs became my second home.",

        "The apartment itself is nice enough and I like the gym, but parking is frustrating and guest parking is basically impossible on weekends.",

        "Moved in because it looked nice on the tour but the wifi cuts out constantly and my roommate found mold behind the kitchen cabinet midway through the semester.",

        "Honestly surprised by how much I liked this place. Quiet, clean, good lighting, and my dog was weirdly happier here than our last apartment.",

        "One night the fire alarm went off three times between midnight and 4am and nobody from management explained what happened. Also saw two cockroaches in the kitchen."
    ]
})

reviews
```

# Word Counts

One of the simplest things we can measure is document length.


```python
reviews["word_count"] = (
    reviews["Review"]
    .str.split()
    .str.len()
)

reviews[["Apartment","word_count"]]
```


```python
reviews.sort_values(by="word_count", ascending=False)
```

# Character Counts


```python
reviews["char_count"] = reviews["Review"].str.len()

reviews[["Apartment","char_count"]]
```

# Most Common Words

Now let’s combine all the reviews together and count the most common
words.


```python
all_words = " ".join(reviews["Review"]).lower().split()

Counter(all_words).most_common(15)
```

Some of these words are useful.

Some are extremely common and do not tell us very much.

# Cleaning Text

Before analyzing text, we usually clean it.

This often includes:

- lowercasing
- removing punctuation
- removing stopwords

## Lowercase


```python
reviews["clean_review"] = (
    reviews["Review"]
    .str.lower()
)

reviews["clean_review"].head()
```

## Remove Punctuation


```python
reviews["clean_review"] = (
    reviews["clean_review"]
    .str.translate(
        str.maketrans("", "", string.punctuation)
    )
)

reviews["clean_review"].head()
```

## Stopwords

Stopwords are very common words like:

- the
- and
- is
- of

These words are often removed because they carry little meaning by
themselves.


```python
from sklearn.feature_extraction.text import ENGLISH_STOP_WORDS

stopwords = set(ENGLISH_STOP_WORDS)
```


```python
reviews["clean_review"] = reviews["clean_review"].apply(
    lambda x: " ".join(
        word for word in x.split()
        if word not in stopwords
    )
)

reviews["clean_review"].head()
```

# Most Common Cleaned Words


```python
clean_words = " ".join(
    reviews["clean_review"]
).split()

Counter(clean_words).most_common(15)
```

# Visualizing Word Counts


```python
common_words = pd.DataFrame(
    Counter(clean_words).most_common(10),
    columns=["word","count"]
)
```


```python
common_words.plot.bar(
    x="word",
    y="count",
    legend=False,
    title="Most Common Words in Apartment Reviews"
)
```

# You Try

Pick one apartment review and answer:

1.  Does the cleaned version preserve the meaning of the original
    review?
2.  What information was removed?
3.  What information became easier to analyze?

# Limits of Word Counts

Consider these two sentences:

- “dog bites man”
- “man bites dog”

A simple word count treats these as almost identical.

But they clearly mean very different things.

Word order matters.

# Folktales Dataset

Now let’s move to a larger and more complicated text dataset.


```python
folktales = pd.read_csv("data/folktales.csv")

folktales.head()
```

# Exploring the Dataset


```python
folktales.columns
```


```python
folktales["Country of Origin"].value_counts().head(10)
```


```python
folktales[["Title", "Author", "Country of Origin", "Story Type"]].head()
```

# Select a Familiar Story

We’ll focus on Cinderella because:

- it is culturally recognizable
- it contains repeated motifs
- it is long enough for analysis
- it has strong thematic vocabulary


```python
cinderella = folktales[
    folktales["Title"].str.contains(
        "Cinderella",
        case=False,
        na=False
    )
]

cinderella[["Title","Country of Origin", "Author", "Story Type"]]
```

# Extract Story Text


```python
story = cinderella.iloc[1]["Story"]

story[:1000]
```

# Clean the Story


```python
story_clean = story.lower()
```


```python
story_clean = story_clean.translate(
    str.maketrans(
        string.punctuation,
        " " * len(string.punctuation)
    )
)
story_clean = " ".join(story_clean.split())

story_clean[:1000]
```


```python
story_clean = " ".join(
    word for word in story_clean.split()
    if word not in stopwords
)

story_clean[:1000]
```

# Word Frequencies


```python
story_words = story_clean.split()

Counter(story_words).most_common(20)
```

These are some of the most common meaningful words in the story.


```python
cinderella_words = pd.DataFrame(
    Counter(story_words).most_common(15),
    columns=["word", "count"]
)
```


```python
cinderella_words.plot.bar(
    x="word",
    y="count",
    legend=False,
    title="Most Common Words in Cinderella"
)
```

# N-grams

Single words can be useful, but phrases can reveal more structure.

An n-gram is a sequence of words.

Today we will focus on:

- bigrams, or 2-word phrases


```python
from nltk import ngrams
```


```python
bigrams = list(
    ngrams(story_words, 2)
)

Counter(bigrams).most_common(15)
```

# Why N-grams Matter

N-grams preserve some word order information.

For example:

- “king's son”
- “touched wand”
- “glass slippers”

can reveal themes that single words might miss.

# TF-IDF

Word frequency tells us what words are common.

But what words make a story unique?

TF-IDF helps identify words that are:

- common in one document
- uncommon across the whole collection

In plain language:

- **TF** asks: What words appear often in this story?
- **IDF** asks: What words are rare across all stories?
- **TF-IDF** asks: What words help make this story distinctive?

This idea is closely related to how search engines and recommendation
systems identify important terms in documents.

# TF-IDF with Scikit-Learn


```python
from sklearn.feature_extraction.text import TfidfVectorizer
```


```python
tfidf = TfidfVectorizer(
    stop_words="english",
    max_features=5000
)
```


```python
matrix = tfidf.fit_transform(
    folktales["Story"].fillna("")
)
```


```python
matrix.shape
```

# Most Important Words in Cinderella


```python
# The dataset contains multiple versions of Cinderella.
# We'll use the French version because it is closer to the version
# most readers recognize today and contains many familiar story elements.
story_index = cinderella.index[1]
```


```python
feature_names = tfidf.get_feature_names_out()

scores = matrix[story_index].toarray()[0]

important_words = pd.DataFrame({
    "word": feature_names,
    "score": scores
})

important_words.sort_values(
    by="score",
    ascending=False
).head(20)
```

# Compare Word Frequency and TF-IDF


```python
top_tfidf = important_words.sort_values(
    by="score",
    ascending=False
).head(15)

top_tfidf.plot.bar(
    x="word",
    y="score",
    legend=False,
    title="Most Distinctive Words in Cinderella by TF-IDF"
)
```

# Comparing Cinderella and Little Red Riding Hood

TF-IDF becomes especially useful when we compare documents.

Let's compare two familiar stories:

- Cinderella
- Little Red Riding Hood

We expect different stories to have different distinctive words.


```python
little_red = folktales[
    folktales["Title"].str.contains(
        "Little Red",
        case=False,
        na=False
    )
]

little_red[["Title", "Country of Origin", "Author", "Story Type"]]
```


```python
# We'll compare against the French version of Little Red Riding Hood
# for the same reason.
little_red_index = little_red.index[1]

```


```python
# Get Cinderella TF-IDF scores
cinderella_scores = matrix[story_index].toarray()[0]

cinderella_tfidf = pd.DataFrame({
    "word": feature_names,
    "score": cinderella_scores
}).sort_values(
    by="score",
    ascending=False
).head(15)

cinderella_tfidf
```


```python
# Get Little Red Riding Hood TF-IDF scores
little_red_scores = matrix[little_red_index].toarray()[0]

little_red_tfidf = pd.DataFrame({
    "word": feature_names,
    "score": little_red_scores
}).sort_values(
    by="score",
    ascending=False
).head(15)

little_red_tfidf
```

## Visual Comparison


```python
cinderella_tfidf.plot.bar(
    x="word",
    y="score",
    legend=False,
    title="Distinctive Words in Cinderella"
)
```


```python
little_red_tfidf.plot.bar(
    x="word",
    y="score",
    legend=False,
    title="Distinctive Words in Little Red Riding Hood"
)
```

# You Try

Choose a different folktale and repeat the TF-IDF analysis.


```python
folktales[["Title", "Country of Origin"]].sample(10)
```


```python
# Change this title fragment to find a different story
title_fragment = "Wolf"

selected_story = folktales[
    folktales["Title"].str.contains(
        title_fragment,
        case=False,
        na=False
    )
]

selected_story[["Title", "Country of Origin", "Author"]].head()
```


```python
# If the search found at least one story, analyze the first match
selected_index = selected_story.index[0]

selected_scores = matrix[selected_index].toarray()[0]

selected_important_words = pd.DataFrame({
    "word": feature_names,
    "score": selected_scores
})

selected_important_words.sort_values(
    by="score",
    ascending=False
).head(20)
```

# Simple Sentiment Analysis

Another common text analysis task is sentiment analysis.

This attempts to estimate whether text is:

- positive
- negative
- neutral

Sentiment analysis is useful, but it is also imperfect. It can struggle
with sarcasm, context, older language, and cultural nuance.


```python
from textblob import TextBlob
```


```python
reviews["sentiment"] = reviews["Review"].apply(
    lambda x: TextBlob(x).sentiment.polarity
)

reviews[["Apartment","Stars","sentiment"]]
```


```python
reviews.plot.bar(
    x="Apartment",
    y="sentiment",
    legend=False,
    title="Estimated Review Sentiment"
)
```

# Sentiment in Folktales


```python
folktales["sentiment"] = folktales["Story"].fillna("").apply(
    lambda x: TextBlob(x).sentiment.polarity
)

folktales[["Title", "Country of Origin", "sentiment"]].sort_values(
    by="sentiment",
    ascending=False
).head(10)
```


```python
folktales[["Title", "Country of Origin", "sentiment"]].sort_values(
    by="sentiment",
    ascending=True
).head(10)
```

# Limitations of Text Analysis

Text analysis is powerful, but imperfect.

Some challenges include:

- sarcasm
- ambiguity
- context
- cultural meaning
- humor
- changing language
- translation
- historical language
- missing metadata

Simple methods often lose important meaning.

# AI and Modern Language Models

Many modern AI systems use ideas related to text analysis.

Older approaches:

- counted words
- measured frequencies
- used simple statistics

Modern AI systems:

- model context
- learn relationships between words
- represent language mathematically
- generate new text

Large Language Models are built on these kinds of ideas, but at much
larger scales.

# AI Assistant Checkpoint

Try asking an AI assistant:

- “Explain TF-IDF in plain English.”
- “Why do we remove stopwords in text analysis?”
- “What meaning is lost when we use word counts?”
- “How could I improve this text analysis?”
- “Why might sentiment analysis fail on folktales?”

# Mini Challenge

Choose one folktale or the apartment review set and investigate it.

Try to:

- clean the text
- count important words
- generate bigrams
- calculate TF-IDF
- visualize something interesting

Then answer:

1.  What pattern did you notice?
2.  What surprised you?
3.  What important meaning might your analysis miss?
