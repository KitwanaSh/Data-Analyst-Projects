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

I started by taking a look at the data and brainstorming what questions i could answer from it. With the dataset (tmbd movies, which is in csv), i was able to retrieve the following questions:
<hr>

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
<hr>

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
The final thing to do after cleaning is to save the cleaned data into a new csv dataset `df.to_csv.('cleaned_data.csv', index = False)` to use futher during the EDA step. __Notice__ the `index = False`, this means I set my new cleaned csv data to not have index with it.
<hr>

### Step 3: Exploratory Data Analysis (EDA)
I is time to use the questions i brainstormed from the beginning and to start answering those questions.
I treated each question one by one so that i can be clear with my researches.
<br>
##### Research Question 1 (Which 10 movie genres have the most revenue)
As the title states, i am looking for he top 10 movie genres with the biggest revenues in the data. First of all the genres column contains multiple movie genres in some of the rows but separeted with '|'. So, i have to use the `pd.split` method i order to get each genre on it own. I used the `iloc` method to get the top 10 and `groupby` method to set the revenue column in descending order then plot a Horizontal barchart. (see the graph in the note book).
<br>
I created a function so i can plot the top 10 movies with the most revenues, profits, budgets, Ratings, and popularity. This function helps me to avoid the repetion of codes
<br>__The code__

```
def plot_most(column, top = 10):
    sort_df = pd.DataFrame(df_m[column].sort_values(ascending=False))[:top]
    sort_df['title'] = df_m['title']
    plt.figure(figsize = (10, 8))
    sns.barplot(x = column, y = 'title', data= sort_df, label = column)
    if (column == 'profit' or column == 'budget' or column == 'revenue' or column == 'popularity', column == 'ratings'):
        plt.xlabel(column.capitalize() + ' $')
    else:
        plt.xlabel(column.capitalize())
    plt.ylabel('')
    plt.title('Top 10 Movies in with the most ' + column.capitalize())
    plt.legend()
```
(see the graph in the note book).
<br>
##### Research Question 2 (What are the top 10 most successful movie directors?)
Here also, the director colums contains multiple values in the same row repareted by '|', so i use the `str.split` function to get each movie director individually. I go ahead the use the groupby method to get the ratings in descenting order then plot the vertical barchart uning the `pandas.iloc` to get the top 10. (see the graph in the note book).
<br>

##### Research Question 4 (What kind of properties are associeted with movies that have high revenue?)
I plot multiple histograms at once to check for the relationship distribution that exists within release_year, budgets, profits, revenues and popularity.
<br>
##### Researach Question 5 (What are the top 10 most frequent genres?)
Here, i just used the pd.value_couns() function to get the dataframe of the top 10 most frequent genres puting the head to (10).
<br>
##### Research Question 6 (What is the ralationship between the budget and the revenue throught the years?)
Since the budget and revenue are both numeric and i am looking for the relationships, i figure i will plot a scatter graph.
<hr>

### Step 4: Share my findinfs.
The results about the above exploration are as follow:

- Movie directors should focus on 1. Action, 2. Adventure, 3. Comedy, 4. Drama, 5. Thriller, 6. Fantasy, 7. Familly, 8. Science Fiction, 9. Romance, 10. and Crime because that what most people whatch.
- The number of movies have been growing povitively from 1960 to 2015. This shows how people are getting to like more movies every year coming. From year to year, the revenue increases, only - between 2002 and 2007, the revenue decrease a bit.
- The revenue doesn't increase because of a movie popularity, ratings and budget. #### Limitations in the Eploratory Data Analysis The minimum of revenue and budget are in 0 dollars. I believe this shouldn't be that way, but i couldn't guess the minimum amount of revenue and budget respectively and i coudn't drop the revenue and budget with the value of 0 dollar neither because there are more that 5000 movies with the values of 0 dollar revenue and budget.

#### Heads-up
Of course this is just a breive of the data analysis, while it is important to go through this steps before taking each data analysis step on its own project, you can easily figure already how to go about the data analysis process. I did not explain each step in details here but you can find the deeper explainations in each single other project.
