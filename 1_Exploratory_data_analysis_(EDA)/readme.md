## Investigate a dataset
<hr>
In this project, i investigate a dataset about 10,000 movies collected from The Movie Database from 1960 to 2015 and then i communicate my findings about it.<br>
I asked questions, gather the data, assess the data, do a little cleaning (eg. revove duplicates and some columns), do a deep exploratory data analysis of this datasets then comuncate my findings. This seems like doing the whole data analysis steps, i agree. The purpose of doing so, is to get the big picture of the data analysis. This is a kind of an introduction to data analysis but each step will be covered on its own project.

### Tools used
I use the python libraries such as, numpy, matplotlib and and pandas to make my analysis easier.
<br>I needed to install python with the above libraries.

### Why this project
In this project, i go through the data analysis process and see how everything fits together because after completing this project, I:
- Know all the steps involved in a typical data analysis process
- Am comfortable posing questions that can be answered with a given dataset and then answering those questions
- Know how to investigate problems in a dataset and wrangle the data into a format you can use
- Have experience communicating the results of your analysis
- Am able to use vectorized operations in NumPy and pandas to speed up your data analysis code
- Am familiar with pandas' Series and DataFrame objects, which let you access your data more conveniently
- Know how to use Matplotlib to produce plots showing your findings
<hr>
I started by taking a look at the data and brainstorming what questions i could answer from it. With the dataset (tmbd movies, which is in csv), i was able to retrieve the following questions:

### step 1: Questions for investigation
- Which 10 movie genres have the most revenue ?
- What are the top 10 most successful movie directors ?
    - Show the number of movies each year.
- what is the relationship between the release year with the ratings ?
- What kinds of properties are associeted with movies that have high revenue ?
- What are the top 10 most frequent genres?
- What is the ralationship between the budget and the revenue throught the years?
    - Which movie had the highest budget ?
    - Which movie had the lowest budget ?
    - Which movie had the highest revenue ?
    - Which movie had the lowest revenue ?
    - How the revenue tends to move with the profit ?

These above questions led me to the wrangling step.

### step 2: Data wrangling
The first thing is to import the necessary libraries for the analyis.
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```
The above code show the importation of libraries to my jupyter notebook. Note that i used the `%matplotlib inline`, this is not a library importation, it is a syntax that allows visuals to be read on the notebook.
<br>
In the wrangling process, i start by `assessing` the dataset, which is about _acquiring_ (gathering) the data (tmdb_movies.csv) and load the first two rows in the notebook by using the pandas methods `pandas.read_csv` and `pandas.head(2)`. __Note__, in the notebook, i use the alias method to alias the the pandas library as *pd*, numpy as *np* and matplotlib.pyplot as *plt*.
Then looking at the data to check for untidy issues from it using pandas and numpy. Some of the assessment method i use are, `df.info(), df.nunique(), df.isna().sum(), df.relase_year.unique(), df.describe`.<br>
The cleaning process was about removing some unnecessary culmns from the dataframe (table) using df.drop, removing null values in the rows using df.dropna(inplace = True), renaming the columns so that the can be easy to identify using the the method df.rename(columsn:{,,}), removing dublicate rows in the dataset and duplicate in the column ['title'] using the method df.drop_duplicates(inplace = True).
<br>
The final thing to do after cleaning is to save the cleaned data into a new csv dataset `df.to_csv.('cleaned_data.csv', index = False)`. __Notice__ the `index = False`, this means I set my new cleaned csv data to not have index with it. 
