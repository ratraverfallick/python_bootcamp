# Data Storytelling, The Pudding, and Your REU Project

 -- Dr. Eric Godat

## Session Overview

Today has two goals:

1. Learn how professional data storytellers build compelling narratives from data.
2. Begin planning your own data storytelling project.

We will start by examining examples from **The Pudding**, then transition into brainstorming and planning for your own project.

---

# The Pudding: Data Storytelling and Visualization

## Overview

Before we begin our own projects, we are going to examine how professional data storytellers build compelling narratives from data.

For this activity we will explore articles from **The Pudding**, a publication that combines journalism, design, visualization, and data science.

Website:

https://pudding.cool/

---

# Learning Goals

By the end of this activity, you should be able to:

- Identify a research question
- Recognize how data supports an argument
- Critically evaluate visualizations
- Understand how analytical choices shape conclusions
- Begin thinking about your own data storytelling project

---

# Part 1: Explore The Pudding

Spend approximately 30-40 minutes reading one article from The Pudding.

Choose something that genuinely interests you.

Possible starting points:

- Music
- Sports
- Movies
- Language
- Culture
- History
- Technology
- Food
- Social Media

The specific topic is less important than understanding how the story is constructed.

---

# Part 2: Analysis Questions

As you read, answer the following questions.

## The Data

### Where did the data come from?

- Public data?
- Survey data?
- Web scraping?
- Original data collection?

### Is the data available publicly?

### How large was the dataset?

### Did they use all of the data or only a subset?

### What limitations did the dataset have?

---

## The Analysis

### What is the main question being asked?

Try to write it in a single sentence.

### What is the thesis statement?

What argument are they trying to make?

### What evidence did they use to support the argument?

### Did the analysis change as the article developed?

### What assumptions did they make?

### What caveats or limitations did they acknowledge?

### Were there alternative explanations?

---

## The Visualizations

### What types of visualizations were used?

Examples:

- Bar charts
- Scatter plots
- Maps
- Timelines
- Interactive graphics
- Network diagrams

### Which visualization was most effective?

Why?

### Did any visualization confuse or distract you?

### Could you recreate a simpler version in Python?

How might you do it?

---

## The Story

### What made the article interesting?

### Would the story have been convincing without the data?

### Did the visualizations strengthen the argument?

### What did you learn?

### What questions remain unanswered?

---

# Part 3: Small Group Discussion

Discuss your article with 2–3 other students.

Focus on:

- Similarities between articles
- Differences in approaches
- Data sources
- Visualization choices
- Storytelling techniques

Be prepared to summarize your discussion.

---

# Part 4: Whole Class Discussion

For each article we will discuss:

1. What question was being asked?
2. What data was used?
3. What was the most effective visualization?
4. What was the biggest limitation?
5. What made the story memorable?
6. What analysis do you think was performed that never appeared in the final article?

---

# Part 5: How Data Stories Are Built

Many successful data stories follow a structure like this:

Question
↓
Data
↓
Analysis
↓
Visualization
↓
Story
↓
Conclusion

Notice that visualization is only one piece of the process.

A beautiful graph cannot rescue a weak question or poor analysis.

Likewise, excellent analysis is often ignored if it is communicated poorly.

---

# Part 6: From Reader to Researcher

Now that we have seen examples of professional data storytelling, it is time to start thinking about our own projects.

Good projects often begin with:

- Curiosity
- Frustration
- Surprise
- Personal interests
- Public-interest questions

Strong projects are not necessarily serious projects.

Some of the best projects begin with questions that seem strange, unexpected, or playful.

---

# Brainstorming Exercise

Spend 10 minutes generating ideas.

Write down:

## Three things you are interested in

Examples:

- Sports
- Transportation
- Housing
- Gaming
- Movies
- Music
- Food
- Public Policy

## Three questions you have

Examples:

- Why are some cities growing faster than others?
- Are book adaptations really worse than the original?
- Which counties have the greatest barriers to food access?

## Three possible datasets

Do not worry if you are unsure whether the data exists.

The goal is to generate possibilities.

---

# Data Storytelling Project

## Overview

For this project, you will create a short data story inspired by the work of organizations such as The Pudding.

The goal is not to build the most advanced model or analyze the largest dataset. Instead, the goal is to investigate an interesting question, use data as evidence, and communicate your findings clearly.

You will have until next Friday to complete the project.

---

# Project Goal

Create an **8-minute presentation** that tells a compelling story using data.

Your project should demonstrate the major stages of the data science workflow:

1. Question Formulation
2. Data Collection
3. Data Cleaning
4. Analysis
5. Visualization
6. Interpretation

---

# What Makes a Good Project?

Strong projects are:

- Interesting
- Focused
- Feasible
- Supported by data
- Personally meaningful

A simple project with a clear story is often stronger than a complicated project that never reaches a conclusion.

---

# Finding a Question

Your first task is to identify a question that you genuinely want to answer.

Examples:

- Which Texas counties appear to have the greatest barriers to food access?
- Are movie adaptations rated lower than the books they are based on?
- What makes one folktale different from another?
- Do college towns have unusual housing markets?
- Which states have the most concentrated populations?
- What factors seem related to economic mobility?

Good questions are specific enough to investigate but broad enough to allow exploration.

---

# Finding Data

Potential sources:

- [Data.gov](https://data.gov)
- [ResearchData.gov](https://www.researchdatagov.org)
- [Census Reporter](https://censusreporter.org)
- [Kaggle](https://www.kaggle.com/datasets)
- [FiveThirtyEight](https://data.fivethirtyeight.com/)
- [UCI Machine Learning Repository](https://archive.ics.uci.edu)
- [Data Is Plural](https://docs.google.com/spreadsheets/d/1wZhPLMCHKJvwOkP4juclhjFgqIY8fQFMemwKL2c64vk/edit?gid=0#gid=0)

You are encouraged to find unusual or unexpected datasets.

---

# Project Constraints

Your project must:

- Investigate a question that can be answered with data
- Use a dataset large enough to support analysis
- Include at least two original visualizations
- Use Python for some portion of the analysis
- Cite all datasets and external sources
- Be reproducible by another student

Projects do not need to use advanced statistics or machine learning.

---

# Suggested Workflow

## Step 1: Question

What do you want to know?

## Step 2: Data

What data might help answer that question?

## Step 3: Cleaning

What problems exist in the data?

Examples:

- Missing values
- Inconsistent formatting
- Duplicate records
- Incorrect values

## Step 4: Analysis

Explore the data and look for patterns.

## Step 5: Visualization

Create figures that support your findings.

## Step 6: Interpretation

What did you learn?

What surprised you?

What limitations should readers know about?

---

# GitHub Repository Requirements

Each student will maintain a project repository within the **SMUREU GitHub Organization**.

At a minimum, the repository should contain:

```text
project-name/
│
├── README.md
├── data/
├── notebooks/
├── figures/
└── presentation/
```

## README.md

Your README should explain:

- The research question
- Why you chose the topic
- The dataset(s) used
- Major findings
- Repository organization

## data/

Contains:

- Raw data (when permitted)
- Cleaned data
- Data documentation

If data cannot be redistributed, include instructions explaining how another researcher can obtain it.

## notebooks/

Contains all analysis notebooks used during the project.

## figures/

Contains exported figures, maps, charts, and visualizations used in your presentation.

## presentation/

Contains your final presentation slides.

---

# Reproducibility

Good research is reproducible.

Another student should be able to:

1. Understand your question
2. Locate your data source
3. Follow your analysis
4. Recreate your visualizations
5. Understand your conclusions

Perfect reproducibility is not required, but your workflow should be understandable and documented.

---

# GitHub Activity

Commit your work regularly throughout the week.

Frequent commits:

- Create backups
- Document progress
- Make collaboration easier
- Provide a record of how your project evolved

---

# Presentation Requirements

Your final presentation should be approximately **8 minutes**.

Suggested structure:

## Slide 1 — Question

What did you want to know?

## Slide 2 — Data

Where did the data come from?

## Slide 3 — Cleaning and Preparation

How did you prepare the data?

## Slides 4–6 — Findings

Present your most important visualizations and results.

## Slide 7 — Caveats

What can your data not tell us?

What assumptions did you make?

## Slide 8 — Takeaways

What did you learn? Why does it matter?

---

# Evaluation Criteria

| Category | Weight |
|-----------|----------|
| Interesting Question | 20% |
| Data & Analysis | 25% |
| Visualizations | 20% |
| Storytelling & Communication | 20% |
| GitHub Repository & Documentation | 15% |
---

# Project Idea Card

Before leaving class, send me in Slack:

1. A question you would like to investigate
2. Why it interests you
3. A possible data source
4. One visualization you might create

This is not a commitment.

It is simply a starting point for your project.

---

# Success Criteria

By the end of next week, you should be able to answer:

> What question did I investigate, what evidence did I find, and what story does the data tell?

Remember:

Data science is not just about finding answers.

It is about asking good questions, gathering evidence, and communicating what you learn.

