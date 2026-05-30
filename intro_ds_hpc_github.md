# Welcome to Data Science Bootcamp
## Session 1: Introduction, M3 WebPortal, and JupyterLab

---

# Welcome to the Summer Data Science Bootcamp

Welcome!

Before we get started, let's go around the room and introduce ourselves.

Please tell us:

- Your name
- Your home institution
- Your major (or intended major)

If you'd like, feel free to also share:

- A research interest
- A hobby
- Something you're hoping to learn this summer

---

Over the next week, we'll be learning some of the tools and techniques that data scientists use to work with real-world data.

We'll be working with:

- Python
- Jupyter notebooks
- GitHub
- data visualization
- census data
- maps
- text data
- data storytelling

The goal is not to memorize a bunch of commands.

The goal is to become comfortable exploring data, asking questions, and figuring things out.

By the end of the bootcamp, you should feel more comfortable:

- working with data
- reading and modifying Python code
- creating visualizations
- using maps and geographic data
- using AI tools to help you learn and debug
- communicating findings through data stories

---

## A Note About Learning to Code

If you've never programmed before, that's completely fine.

If you've programmed before, you'll probably still encounter things that are new.

Programming is less about knowing the answer immediately and more about learning how to investigate problems.

A typical workflow looks something like this:

1. Try something
2. Get an error
3. Read the error
4. Search for information
5. Ask for help
6. Fix the problem
7. Repeat

That process is normal.

In fact, it is how most professional programmers work.

---

## Using AI During the Bootcamp

You are encouraged to use AI tools during this bootcamp.

Good uses include:

- Explaining concepts
- Debugging code
- Understanding error messages
- Brainstorming approaches
- Exploring alternative solutions

Like any tool, AI can be wrong.

Part of becoming a good data scientist is learning when to trust a result and when to verify it yourself.

---

# What Counts as Data?

Let's start with a simple question:

> What counts as data?

Take a minute and think about examples.

We'll collect ideas on the board.

---

### Whiteboard Activity

As a group, brainstorm examples of things that could be considered data.

Possible categories include:

| Category | Examples |
|-----------|-----------|
| Numbers | Census records, sports statistics |
| Text | Books, tweets, survey responses |
| Images | Photographs, satellite imagery |
| Maps | Counties, roads, GPS tracks |
| Audio | Music, podcasts |
| Video | YouTube videos, security footage |
| Time Series | Weather observations, stock prices |

---

### Discussion

Notice that almost none of these look the same.

Some are numbers.

Some are words.

Some are maps.

Some are images.

Yet all of them can be used to answer questions.

In fact, we'll work with several of these during the bootcamp:

- Census data
- Maps
- Text
- Visualizations

Even though they look very different, we'll approach them using many of the same tools and techniques.

---

# What is Data Science?

There are many different definitions of data science.

For this bootcamp, we'll use a simple one:

> Data science is the process of using data to answer questions.

Questions about:

- people
- cities
- economics
- transportation
- literature
- sports
- history
- social media
- the natural world

can all be approached using data.

---

One useful way to think about data science is as a process:

Question

↓

Data

↓

Analysis

↓

Visualization

↓

Story

---

We start with a question.

We gather data that might help answer that question.

We analyze the data to look for patterns.

We visualize the results to make them easier to understand.

Finally, we communicate what we found.

---

### Discussion

Think of a question that interests you.

- What data might help answer it?
- How would you collect it?
- What challenges might you encounter?

There is no single correct answer.

---

# The Tools We'll Use

Throughout this bootcamp we'll be using four primary tools:

1. M3 WebPortal
2. JupyterLab
3. GitHub
4. AI assistants

Each tool serves a different purpose.

| Tool | Purpose |
|--------|---------|
| M3 WebPortal | Access our computing environment |
| JupyterLab | Write and run Python code |
| GitHub | Save and track work |
| AI Assistants | Help explain concepts and debug code |

For today, our goal is simply to get everyone connected and running code.

---

# Why Are We Using M3?

This bootcamp could be taught by having everyone install Python on their own laptop.

Instead, we're using M3 because:

- Everyone gets the same software
- Everyone gets the same environment
- We avoid installation problems
- The tools will still be available after the bootcamp
- Many research projects at SMU use M3

---

## What is M3?

M3 is SMU's High Performance Computing (HPC) system.

Rather than requiring everyone to install software on their own computer, we'll all work in the same shared computing environment.

For this bootcamp, you can think of M3 as:

Your Laptop

↓

Web Browser

↓

M3 WebPortal

↓

JupyterLab

↓

Python

This gives everyone access to the same software and the same environment.

---

# Getting Access to M3

Before we can use the M3 WebPortal, we need to make sure our accounts are fully activated.

Open:

https://hpcaccess.smu.edu

Log in using your SMU credentials.

You should already be enrolled in the bootcamp project.

---

## Accepting the User Agreement

The first time you log in, you may be asked to review and accept the M3 user agreement.

For each project allocation:

☐ Review the policies

☐ Accept the policies

☐ Confirm your participation

Once completed, your HPC account will be activated.

> Note: If you skip this step, you may not be able to launch JupyterLab later.

---

## Launching the M3 WebPortal

Once your account is active:

1. Open the M3 WebPortal - [https://hpc.m3.smu.edu](https://hpc.m3.smu.edu)
2. Navigate to Interactive Apps
3. Select JupyterLab
4. Launch a session
5. Open the JupyterLab interface

---

## Checkpoint

By the end of this section you should have:

☐ Logged into ColdFront

☐ Accepted the user agreement

☐ Verified your project allocation

☐ Opened the M3 WebPortal

☐ Launched JupyterLab

☐ Opened a notebook

---

## Tiny Victory

Create a new notebook and run:

```python
print("Hello Data Science!")
```

Then try:

```python
2 + 2
```

Congratulations!

You have successfully connected to M3 and executed your first Python code.

---

## Next Up

Now that everyone is working in the same environment, we'll learn how Jupyter notebooks actually work.

# Jupyter Notebook Survival Skills

Throughout this bootcamp, almost all of our work will happen inside Jupyter notebooks.

Jupyter notebooks allow us to combine:

- Code
- Text
- Visualizations
- Notes
- Results

into a single document.

This makes them one of the most common tools used in data science and research.

---

## Cells

Jupyter notebooks are made up of cells.

There are two primary types of cells:

### Markdown Cells

Markdown cells are used for:

- Notes
- Explanations
- Documentation
- Instructions

This cell is a Markdown cell.

### Code Cells

Code cells contain Python code.

When we execute a code cell, the result is displayed directly below the cell.

---

## Running Cells

There are several ways to run a cell.

The most common is:

**Shift + Enter**

This:

- Executes the current cell
- Moves to the next cell

You can also use the **Run** button in the toolbar.

---

## Exercise: Running Code

Try executing the following cells.

```python
3 + 4
```

```python
10 * 5
```

```python
100 / 4
```

---

## Variables

Variables allow us to save information for later use.

Think of a variable as a labeled box.

We can put something into the box and retrieve it later.

```python
favorite_number = 7
```

```python
favorite_number
```

```python
favorite_number + 10
```

---

## Notebook State

A very important concept in Jupyter notebooks is **state**.

When you run a cell, the notebook remembers the variables and objects that were created.

```python
favorite_number = 7
```

```python
favorite_number
```

Where did Python get the value of `favorite_number`?

It remembered it from the previous cell.

---

## The Kernel

Behind every notebook is a Python process called the **kernel**.

The kernel is responsible for:

- Running code
- Storing variables
- Managing memory

When you execute a cell, you are sending instructions to the kernel.

---

## Demonstration: Restarting the Kernel

Run the following cell:

```python
my_name = "Eric"
```

Then run:

```python
my_name
```

You should see:

```python
'Eric'
```

Now restart the kernel using:

```text
Kernel → Restart Kernel
```

After the restart, run only:

```python
my_name
```

What happened?

The notebook still exists.

The code still exists.

But the kernel forgot everything stored in memory.

This is one of the most common sources of confusion for new Python users.

---

## Execution Order Matters

Run:

```python
x = 10
```

Then:

```python
x + 5
```

You should get:

```python
15
```

Now change the first cell to:

```python
x = 100
```

Do **not** run it.

Run only:

```python
x + 5
```

Why didn't the answer change?

Because the notebook text and the kernel memory are not necessarily synchronized.

Changing a cell does not change memory until that cell is executed.

---

## A Useful Habit

When a notebook starts behaving strangely:

1. Save your work
2. Restart the kernel
3. Run all cells

This solves a surprising number of notebook problems.

---

## Saving Your Work

Jupyter notebooks save automatically.

However, it is still a good habit to save your work:

- Before major changes
- Before closing your browser
- Before logging out
- Before your M3 job session expires

Interactive sessions on M3 have time limits.

If your session ends, unsaved work may be lost.

When in doubt, save your notebook.

### Professional Tip

Save early.

Save often.

Every experienced programmer has lost work at some point.

---

## Useful Shortcuts

A few shortcuts that are helpful to know:

| Shortcut | Action |
|-----------|-----------|
| Shift + Enter | Run current cell |
| A | Insert cell above |
| B | Insert cell below |
| D, D | Delete current cell |

To use these shortcuts, first click outside the cell so that the cell border changes color.

---

## Mini Challenge

Create two variables:

- Your first name
- Your favorite food

Then use them in a print statement.

For example:

```python
name = "Eric"
food = "Tacos"

print(name, "likes", food)
```

Try modifying the values to describe yourself.

---

## Checkpoint

By this point you should be able to:

☐ Create a notebook

☐ Run a code cell

☐ Create variables

☐ Understand what the kernel does

☐ Restart the kernel

☐ Save your notebook

☐ Recover from common notebook issues

These skills will be used throughout the remainder of the bootcamp.

# Saving and Sharing Work with GitHub

Throughout this bootcamp we will use GitHub to:

- Access course materials
- Save our work
- Track changes over time
- Share projects
- Collaborate with mentors and other researchers

GitHub will not just be used during this bootcamp.

Throughout the summer, your research projects will live within the SMUREU GitHub organization.

---

## GitHub Accounts

If you do not already have a GitHub account:

https://github.com

Create an account and choose a professional username.

If you already have a GitHub account, make sure you can log in.

Share your GitHub username with Dr. Godat so you can be added to the SMUREU organization.

---

## Joining the SMUREU Organization

Once you receive the organization invitation:

1. Open GitHub
2. Accept the invitation to the SMUREU organization
3. Verify that you can access SMUREU repositories

---

## A Tiny Bit of Terminal

Inside JupyterLab, open a terminal from the Launcher page.

Today we only need a few commands:

| Command | Purpose |
|---|---|
| `pwd` | Show current location |
| `ls` | List files and folders |
| `cd folder_name` | Move into a folder |
| `cd ..` | Move up one level |
| `git clone` | Download a repository |
| `git status` | Show changes |
| `git add` | Stage changes |
| `git commit` | Save a snapshot |
| `git push` | Send changes to GitHub |

Try:

```bash
pwd
```

```bash
ls
```

---

## Set Your Git Identity

Before making commits, tell Git who you are.

Use the same name and email associated with your GitHub account.

```bash
git config --global user.name "Your Name"
```

```bash
git config --global user.email "your_email@example.com"
```

You can check your settings with:

```bash
git config --global --list
```

---

## Set Up SSH Access

GitHub needs a way to verify who you are when you clone or push repositories from M3.

Because we are working from M3, we will use SSH keys.

Think of an SSH key as a digital ID card.

Check whether you already have an SSH key:

```bash
ls ~/.ssh
```

If you see files named something like:

```text
id_ed25519
id_ed25519.pub
```

you may already have a key.

If you do not have a key, generate one:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

When prompted, accept the default file location by pressing Enter.

You may also press Enter when asked for a passphrase if you do not want to set one for this workshop.

Display your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output.

It should begin with something like:

```text
ssh-ed25519
```

---

## Add Your SSH Key to GitHub

In GitHub:

1. Open **Settings**
2. Select **SSH and GPG keys**
3. Click **New SSH key**
4. Give it a title like `M3 WebPortal`
5. Paste your public key
6. Save

---

## Test Your GitHub Connection

Run:

```bash
ssh -T git@github.com
```

You may be asked whether you want to continue connecting.

Type:

```bash
yes
```

You should see a message similar to:

```text
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

That means M3 can now communicate with GitHub.

---

## Fork the Course Repository

The bootcamp materials live here:

https://github.com/SMUREU/python_bootcamp

Rather than modifying the original repository directly, you will create your own copy.

This process is called a **fork**.

1. Open the course repository in GitHub
2. Click **Fork**
3. Create the fork under your own GitHub account
4. Verify that the new repository appears under your profile

After this step there should be two repositories:

```text
SMUREU/python_bootcamp
```

and

```text
YOUR_USERNAME/python_bootcamp
```

---

## Clone Your Fork

Now download your fork onto M3.

Replace `YOUR_USERNAME` with your GitHub username.

```bash
git clone git@github.com:YOUR_USERNAME/python_bootcamp.git
```

Move into the repository:

```bash
cd python_bootcamp
```

List the files:

```bash
ls
```

You should see the notebooks and datasets that we will use throughout the bootcamp.

---

## Make Your First Change

Create a simple notes file:

```bash
echo "# Notes from Day 1" > my_notes.md
```

Check Git's status:

```bash
git status
```

Git should notice that a new file has been created.

---

## Save a Snapshot

First, tell Git which file we want to save:

```bash
git add my_notes.md
```

Now create a commit:

```bash
git commit -m "Added Day 1 notes"
```

A commit is a snapshot of your project at a specific moment in time.

---

## Push Your Changes to GitHub

Send your commit to GitHub:

```bash
git push
```

Refresh your repository page in GitHub.

You should see your new file appear.

---

## The GitHub Workflow

For most of this summer, your workflow will look like this:

```text
Edit
 ↓
git status
 ↓
git add
 ↓
git commit
 ↓
git push
```

You now know enough Git and GitHub to participate in the bootcamp and begin managing your summer research repository.

# Using AI as a Research Assistant

Throughout this bootcamp you are encouraged to use AI tools.

AI can be a powerful learning tool when used thoughtfully.

Some examples include:

- Explaining concepts
- Debugging code
- Understanding error messages
- Brainstorming ideas
- Learning unfamiliar libraries
- Exploring alternative approaches

Like any tool, AI is not perfect.

It can:

- Make mistakes
- Invent information
- Produce code that does not work
- Give incomplete explanations

Part of becoming a good data scientist is learning when to trust a result and when to verify it yourself.

---

## Good AI Questions

Instead of asking:

> Do my homework.

Try asking:

- What does this error message mean?
- Can you explain this code line by line?
- Why does this plot look strange?
- What are some alternative approaches?
- Can you show me an example?
- What should I search for to learn more?

The quality of the answer often depends on the quality of the question.

---

## Activity: Debugging with AI

Run the following code:

```python
numbers = [1,2,3,4,5]

for n in numbers
    print(n)
```

You should receive an error message.

### Your Task

Copy the error message and paste it into your preferred AI tool.

Ask:

- What does this error mean?
- How do I fix it?
- Why did Python produce this error?

Discuss your results with the group.

---

## AI and Research

Throughout the summer, AI may help you:

- Learn new methods
- Understand unfamiliar code
- Explore datasets
- Draft documentation
- Brainstorm analyses

However:

> You remain responsible for your work.

Always verify results before presenting them, publishing them, or including them in a report.

A useful mindset is:

> Trust, but verify.

---

## When AI Works Best

AI tends to be most helpful when:

- You are learning something new
- You are stuck on a specific problem
- You need an explanation from a different perspective
- You need help interpreting an error message

AI tends to be less helpful when:

- You ask vague questions
- You need highly specialized domain knowledge
- You need guaranteed correctness
- You blindly accept its answers

Learning to use AI effectively is a valuable skill in its own right.

---

# Looking Ahead

Throughout the week we'll work with:

- Census data
- Data visualization
- Maps
- Text analysis
- Data storytelling

We'll learn how to turn raw data into evidence that can help answer interesting questions.

Many of the same tools and ideas will reappear throughout the week.

Each session builds on the skills from the previous one.

---

## Reflection

Create a new Markdown cell in your notebook and answer the following questions.

1. What topic interests you most right now?
2. What data might exist about that topic?
3. What is one question you would like to answer this summer?
4. What is one thing from today that was new to you?

There are no right or wrong answers.

The goal is simply to start thinking like a data scientist:

> What questions interest me, and what data might help answer them?

---

# End of Session Checklist

By the end of today you should have:

☐ Logged into M3

☐ Accepted the user agreement

☐ Launched JupyterLab

☐ Created a notebook

☐ Learned how notebook kernels work

☐ Joined the SMUREU GitHub organization

☐ Forked the bootcamp repository

☐ Cloned a repository

☐ Made a commit

☐ Pushed changes to GitHub

☐ Used AI to help solve a problem

You now have all of the tools needed for the remainder of the bootcamp.

This afternoon we begin working with real datasets in Python.