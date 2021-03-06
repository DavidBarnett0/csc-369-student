---
jupyter:
  jupytext:
    encoding: '# -*- coding: utf-8 -*-'
    formats: ipynb,md,py
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.8.0
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

<!-- #region slideshow={"slide_type": "slide"} hideCode=false hidePrompt=false -->
# Chapter 1 - Just a little Bash of distributed computing?

## Distributed Computing Warmup

Paul E. Anderson
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
## Ice Breaker

What's your best holiday gift ever?
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
While this text can be viewed as PDF, it is most useful to have a Jupyter environment. I have an environment ready for each of you, but you can get your own local environment going in several ways. One popular way is with Anaconda (<a href="https://www.anaconda.com/">https://www.anaconda.com/</a>. Because of the limited time, you can use my server.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
## Preamble
Hello and welcome to an introduction to distributed computing. While there are many ways to approach this subject from both a practical and historical perspective, we are restricting ourselves to a view of distributed computing that attempts to build efficient solutions to typical problems that require distributed computing. For the majority of this class this involves solutions that relate to the data science pipeline:
1. Obtaining data
2. Scrubbing data
3. Exploring data
4. Modeling data
5. Interpreting data
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
* We will not and cannot attempt to cover data science in addition to distributed computing. 
* We will view it as one of the primary applications that require distributed computing. 
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Shorest of Short History Lessons
For our discussion, the world did not exist prior to the 1980s. So our story begins with the rise of the client/server model connected by the internet. This is our most familiar distributed system, and it is shown below:

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c9/Client-server-model.svg/1200px-Client-server-model.svg.png" width=300>

While an entire distributed system course could be taught around different aspects of this picture, we will again refocus back to data science. In other words, we view distributed systems as a tool to help us solve data science and related activities. As most of the world is now data-driven, this is not a stretch for many backgrounds even if your never destinated to be a data scientist or data engineer.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Distributed Computing

As is the case in many fields, the term distributed computing has varied defintions. For this course, we will discuss both loosely coupled distributed systems and tightly coupled distributed systems. The terms "distributed computing", "parallel computing", and "concurrent computing" all have some overlap though distinctions are often made in context. An example of a loosely coupled distributed system is the client-server model shown. An example of a tightly coupled distributed system is performing a parallel computation on two cpus (or cores) in a single computer. 
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### So what history is relevant to this book?
<img src="https://s2.studylib.net/store/data/014193816_1-c992dbd11a019db364ebc6c5cbc55e2d.png" width=700>
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Never tell me the abstractions!

<img src="https://media.tenor.com/images/0795d63faba1aeb2348eed9d24c78bc6/tenor.png" width=500>
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Fallacies of distributed computing
We will ignore when it is convienent the following falacies:
1. The network is reliable
2. Latency is zero
3. Bandwidth is infinite
4. The network is secure
5. Topology does not change
6. There is one administrator
7. Transport cost is zero
8. The network is homogenous

Source: Arnon Rotem-Gal-Oz, Fallacies of Distributed Computing Explained,
white paper, http://www.rgoarchitects.com/Files/fallacies.pdf.


<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Why distributed computing?

The first answer to this question is what do you mean by distributed. Everyone thinks of CPU advances over the years, but don't forget other hardware advances have arrived:

<img src="https://techtalk.pcmatic.com/graph_lib/research/rc_mem_avgmem_pctype.php" width=300>

It is very important to keep in mind that a problem that needed one form of distributed computing in the past, may not need the same form of distributed computing today. 

Our answer to this question is:

**We build distributed systems to build more efficient and optimized solutions to solve problems of interest.**
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### How are distributed systems different?
While we can abstract away some of elements of distributed computing, we are going to study approaches for:
1. How to store data on multiple systems?
2. How to handle updates and fix (or handle) inconsistencies?
3. How do we assemble the full answer?
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Practical Considerations
Most real world examples that need distributed computing need distributed computing because they would otherwise (and may still) require a long time to run. This isn't practical for learning. More importantly to remember, even in the real world we test on small subsets of data before scaling up. All of the examples throughout are scaled down representations of a real problem that may require distributed computing depending on time and resources.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Parallel Processing Logs
Consider the cybersecurity task of examining web server logs. Specifically, read/skim this article in groups of three:
<a href="https://resources.infosecinstitute.com/topic/log-analysis-web-attacks-beginners-guide/">Log Analysis</a>.

Once you have read the article, consider the problem of running Scalp on a single log. Probably not an issue. Consider running it on a very large log (web server logs can get very very big). 

Because they can get big they are often archived routinely. So now you have a problem of looking through many different log files. We may want to run Scalp on the logs over a long period of time. We may want to run it with different parameters. We may make mistakes and need to run it again quickly. You are starting to get my point I think.

Would this problem need distributed computing. Discuss in your groups and bring the answer back in a few minutes.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
It boils down to these questions for us over and over again (some of these overlap):
* Do I need a more efficient (i.e., faster) approach?
* Can my task be broken down into components that may be analyzed and then combined?
* Is it worth the effort? 
* Can you just wait for this answer?
* Can you estimate how long a non-distributed approach will take?
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Stop and consider
We will be jumping straight into command line usage examples. Dependening on your comfort level you may want to flip down to the **"Detour: Linux and Bash"** section. 
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Example: Project Gutenberg
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
Consider another example. What if you were interested in looking for patterns in the top 100 books last year? Your first idea is to compare the word frequencies in the top 25 books to the next 25 books. 
* You have a level of programming skill that makes writing a Python program to look through a single book (text file) within range. 
* You are familiar with Python dictionaries and can sequentially process a book. 

Let's design such a program together!
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
There is a list of books downloaded from Project Gutenberg in the data folder
* Website has over 60,000 books. 
* I downloaded the most popular books on 1/12/2021 
* The order in which they are ranked is in order.txt. 

Let's see a few Bash commands to take a look at the books.
<!-- #endregion -->

```python slideshow={"slide_type": "fragment"}
!ls ../data/gutenberg
```

```python slideshow={"slide_type": "subslide"}
!ls -l ../data/gutenberg | wc -l
```

```python slideshow={"slide_type": "subslide"}
!head ../data/gutenberg/order.txt
```

<!-- #region slideshow={"slide_type": "subslide"} -->
**Python code to create a list of files in the ranked order**
<!-- #endregion -->

```python slideshow={"slide_type": "fragment"}
from os import path
book_files = []
for book in open("../data/gutenberg/order.txt").read().split("\n"):
    if path.isfile(f'../data/gutenberg/{book}-0.txt'):
        book_files.append(f'../data/gutenberg/{book}-0.txt')
book_files[:10]
```

<!-- #region slideshow={"slide_type": "subslide"} -->
### What is our top book?
<!-- #endregion -->

```python slideshow={"slide_type": "fragment"}
!head {book_files[0]}
```

<!-- #region slideshow={"slide_type": "subslide"} -->
### What is our second book?
<!-- #endregion -->

```python slideshow={"slide_type": "fragment"}
!head {book_files[1]}
```

<!-- #region slideshow={"slide_type": "subslide"} -->
### Partner activity
Finish the following code block (or what you can of the code block within the time limit). Choose someone to drive this time and then we'll swap next time. We want to know the frequencies of the words in each book. One way to store this is in a dictionary for each book. Do not worry about punctuation or capitalization. This is just an exercise.
<!-- #endregion -->

```python slideshow={"slide_type": "subslide"}
def count_words_book(book):
    file = open(book).read()
    book_word_freq = {}
    # YOUR SOLUTION HERE
    return book_word_freq

def count_words(book_files):
    book_word_freq = {}
    for book in book_files:
        book_word_freq[book] = count_words_book(book)
    return book_word_freq
```

```python slideshow={"slide_type": "subslide"}
book_word_freq = count_words(book_files)
```

<!-- #region slideshow={"slide_type": "fragment"} -->
**If you want to time it**
<!-- #endregion -->

```python slideshow={"slide_type": "fragment"}
%%timeit -n 1
book_word_freq = count_words(book_files)
```

<!-- #region slideshow={"slide_type": "subslide"} -->
## GNU Parallel
One of my favorite command line finds of all time: <a href="https://www.gnu.org/software/parallel/">https://www.gnu.org/software/parallel/</a>
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
Our small example on <100 books did not take very long. This wouldn't classify as something that needs distributed computing. But consider how likely it is that instead of counting words, we are doing something more computationally intensive. OR consider that we may be doing something simple like counting words, but instead of a few books, it is the internet itself...

My point is that depending on the application you may want to take something you've written and run it in parallel. While there are language extensions for this, we are focusing on command line distributed computing execution at the moment. To show you how easy this is, you'll need to put your code into a Python script:
<!-- #endregion -->

```python slideshow={"slide_type": "subslide"}
# Make sure you change \n to \\n in your code
code = """
import sys

def count_words_book(book):
    file = open(book).read()
    book_word_freq = {}
    # YOUR SOLUTION HERE
    return book_word_freq
    
book = sys.argv[1]
count_words_book(book) # I am not printing the output on purpose because we are timing this.
"""
open("count_words_book.py",'w').write(code)
```

```python slideshow={"slide_type": "subslide"}
!head -n 5 count_words_book.py
```

```python slideshow={"slide_type": "skip"}
!sudo apt-get install -y parallel
```

<!-- #region slideshow={"slide_type": "subslide"} -->
### Running parallel is as simple as
* Knowing how to pipe a list of files to a program
* That program must take a file as an argument
<!-- #endregion -->

```python slideshow={"slide_type": "subslide"}
!find ../data/gutenberg -name "*.txt" | egrep -v order.txt | parallel echo %
```

```python slideshow={"slide_type": "subslide"}
%%timeit -n 1
!find ../data/gutenberg -name "*.txt" | egrep -v order.txt | parallel python count_words_book.py
```

<!-- #region slideshow={"slide_type": "subslide"} -->
### Results
* We got a speedup even though we had to start Python multiple times (our results may vary on a shared environment). 
* There is always overhead when moving towards distributed computing. 
* We aren't going to do the actual comparison of top 25 to next 25. 
* Well... why not. We are almost there. This material is definitely not part of this class though.
<!-- #endregion -->

```python slideshow={"slide_type": "subslide"}
import pandas as pd
import altair as alt

book_word_freq = count_words(book_files)

top_books = book_files[:25]
next_books = book_files[25:50]

top_df = pd.DataFrame(columns=["book","word","freq","group"]).set_index(["book","word"])
next_df = pd.DataFrame(columns=["book","word","freq","group"]).set_index(["book","word"])

# Normalize the counts for each book
for book in top_books:
    data = pd.Series(book_word_freq[book])
    data = (data/data.sum()).to_frame().reset_index()
    data.columns=["word","freq"]
    data["book"] = book
    data["group"] = "top"
    top_df = top_df.append(data.set_index(["book","word"]))
    
for book in next_books:
    data = pd.Series(book_word_freq[book])
    data = (data/data.sum()).to_frame().reset_index()
    data.columns=["word","freq"]
    data["book"] = book
    data["group"] = "bottom"
    next_df = next_df.append(data.set_index(["book","word"]))
```

```python slideshow={"slide_type": "subslide"}
next_df = next_df.reset_index()
top_df = top_df.reset_index()
```

```python slideshow={"slide_type": "subslide"}
plot_df = top_df.append(next_df)
plot_df
```

```python slideshow={"slide_type": "subslide"}
pivot_df = plot_df.groupby(['word','group']).mean().reset_index().pivot_table(index='group',columns='word').T
```

```python slideshow={"slide_type": "subslide"}
top_k = 30
top_words = [v[1] for v in (pivot_df["bottom"] - pivot_df["top"]).abs().sort_values(ascending=False)[:top_k].index]
top_words
```

```python slideshow={"slide_type": "subslide"}
alt.Chart(plot_df.set_index('word').loc[top_words].reset_index()).mark_bar().encode(
    x='group',
    y='freq',
    column='word',
    color='group'
)
```

<!-- #region slideshow={"slide_type": "subslide"} -->
**Anyways...** I feel like we've gotten that out of our system. Now back to distributed computing.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Wrapping up our warmup
There is so much more to Parallel then we can discuss here. 

For example, you can use Parallel to execute commands on multiple nodes: <a href="https://www.gnu.org/software/parallel/parallel_tutorial.html#Remote-execution">https://www.gnu.org/software/parallel/parallel_tutorial.html#Remote-execution</a>. 

Parallel is one of the most useful distributed computing tools at your disposal. If you have a command line program that would benefit from running in a distributed fashion. Do NOT rewrite it until you have considered running it this way.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Detour: Linux and Bash

While there are many tutorials and introduction to Bash, I like this one: https://ubuntu.com/tutorials/command-line-for-beginners. You may do almost the entire tutorial directly in this notebook. There are several ways to run Bash within Jupyter. Here are some examples.
<!-- #endregion -->
