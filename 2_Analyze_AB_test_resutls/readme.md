## Analyze A/B Test Reesults
<hr>
For this project, I will be working to understand the results of __an A/B test run by an *e-commerce website*__. The company has developed a new web page in order to try and increase the number of users who "convert," meaning the number of users who decide to pay for the company's product. My goal is to work through this notebook to help the company understand if they should implement this new page, keep the old page, or perhaps run the experiment longer to make their decision.
The dataset contains these tables:
- user_id
- timestamp
- group
- landing_page
- converted
<br>
But before we get started, let's define what A/B test is.<br>
__A/B test__ is the process of testing <span style="color:red">two variations of the same web page</span> to determine which page is more successful at attracting user traffic and generating revenue.
<br>
The use case of this project is to learn how to analyze the data using probability and statistics. The project is divided into 3 parts:
1. The probability
2. A/B test
3. Regression approach
<br>
So, before any further do, let's dive into each part.
<hr>

### Part I: PRobability
The first action in this part, which will envolve the other parts too, is to import the necessary libraries for the project. Thos libraries are `numpy`, `pandas`, `random` and `matplotlib` and i set the seed to assure i get the same answer on quizes as i set up (the setting is `random.seed(42)`).
<br>
The second thing is to load the data so we can start answering the probaility question. To do this, i loaded the dataset (`ab_data.csv`) and read the first 5 rows using the function `DataFrame.head()`. The probability phase here is more like the assessment step, only here we looking at the specific informations that relate statistic.<br>
Primarily, i look at:
- The number of rows in the dataframe. code: `df.shape`
- The number of unique users. code: `df.user_id.nunique()`
- The porstion of converted users. code: `df.converted.mean()`
- The number of times new_page and treatement don't line up. Code: `df[((df['group'] == 'treatment') == (df['landing_page'] == 'new_page')) == False].shape[0]`
- If the there is any missing values in the dataset. code: `df.isnull().any()`
<br>
Qn. Another thing to check is, the rows where __treatment__ is not aligned with __new_page__ or __control__ is not aligned with __old_page__. To do this I copy the old dataframe to an new one that i name (df2) then i create a dataframe using the copied Dataframe that meet the specifications of the question.
<br>
Code:
```
-- create the dataframe --
df2 = df[((df.group=='treatment') & (df.landing_page=='new_page')) | ((df.group=='control') & (df.landing_page=='old_page'))]

-- check if the correct rows were removed --
df2[((df2['group'] == 'treatment') == (df2['landing_page'] == 'new_page')) == False].shape[0]
```
It shoud give __0__ if well checked.
After checking i find that there is a user_id repeated, so i remove it.
<br>
The final step in the probability part is to answer the key quetion and to give a conclusion accordingly.
So i answer the following probability questions:
- What is the probability of an individual converting regardless of the page they receive?
- Given that an individual was in the __control__ group, what is the probability they converted?
- Given that an individual was in the treatment group, what is the probability they converted?
- What is the probability that an individual received the new page?
- Then i consider the above questions to explain below whether i think there is sufficient evidence to say that the new treatment page leads to more conversions.
<br>
My explanation is stated like this __The probability of an induvidual converting regardless of the page they are in is 0.1196, for an individual coverting in control group is 0.1204 and for an induividual converting in treatment group is 0.1188. By observing the conversion rate which is 0.0015 (almost to 0) and the probability that an individual received the new_page which is 0.5, we can conclude that there is no difference between old and new landing page in conversion. Thus, since the conversion rate is almost equal to zero (0), we should think about considering other facts.__
<hr>

### Part II: A/B Test
This is where the magic happen, I consder using the statistical to make decision base on all the data i have. I use the type I error of 5% for the hypotesis to decide if the the company should choose the __new page__ over the __old page__ one.
I state my hypothesis in terms of words here, but i could also state in terms of __Pnew__ and __Pold__. Here is how i stated:

__H0: There is no statistically significant difference between sample distribution and theoretical normal distribution__

__H1: There is statistically significant difference between sample distribution and theoretical normal distribution.__ <br>
The test rejects the null hypotesis when the `p-value` is less than or equal to `0.5`. Failing to reject the null hypothesis will allow us to state that we are `95% confident` that the data does not fit the normal distribution.
<br>
Based on the hypothesis questions, i was able to get the following:
- the convert rate for __pnew__ under the null = 0.11959667567149027
- the convert rate for __pold__ under the null = 0.11959667567149027
- the probability under the null = 0.11959667567149027
- nnew (number of quereis where landing page is `new_page`) = 145311
- nold (number of queries where landing page is `old_page`) =145274
I also plot a histogram to plot the sampling distribution of differences and the number of pages and show where the redline lands. To do this i first Simulate 10,000 pnew - pold values above. Store all 10,000 values in a numpy array called `p_diffs`. Before then, the following function was create to have a sampling destribution for the difference in new page and old page for the mean:

```
p_diffs = []
for i in range(10000):
    
    p_new = np.random.choice([1, 0],n_new)
    p_old = np.random.choice([1, 0],n_old)
    p_new_mean = p_new.mean()
    p_old_mean = p_old.mean()
    p_diffs.append(p_new_mean-p_old_mean)
```
And find the proportion of the __p_diffs__ are greater than the actual difference observed in __ab_data.csv__; the answe is 0.8018. This is the p-value (0.8018). This means that we are 80% confident that the new page does not give better results than the old page. __There fore we should not reject the null hypothesis__.
<br>
I also used the stats.proportions_ztest method to compute your test statistic (z_score) and p-value for the number of new and old converted users. They are 1.3116075339133115, 0.905173705140591, z_score and p-value respectively then calculated how significant the z_score is (0.905173705140591) and check critical value of 95% confidence (1.6448536269514722). For this information i conclude that the `z_score` is less than the `95% confidence`. Thus, __we fail to reject the null hypotesis__. Therefore, the conclusion is the same as the previous, we fail reject the null hypothesis.
<hr>

### Part III: A regression approach
This is another approach we can use in statistics. The most efficient and effectie for most scientists. It is the same as using the A/B test.<br>
Since there are only two options (each row is either conversion or no conversion), i will be performing the __'Logistic Regression'__.
<br>
The goal is to use `statsmodels` to fit the regression model to see if there is a significant difference in conversion based on which page a customer receives. However, I first need to create a column for the intercept, and create a dummy variable column for which page each user received. I Add an `intercept` column, as well as an ab_page column, which is 1 when an individual receives the treatment and 0 if control. So i used te following code.
```
df2['intercept'] = 1

df2['ab_page'] = pd.get_dummies(df2['group'])['treatment']
df2.head()
```
The i use the statmodel to import my regression model, initiate the model and fit the model to predict whether or the an individual converts. And I provive a summary statistics. (the in the note book).
<br>
Now along with testing if the conversion rate changes for different pages, i also add an effect based on which country a user lives. I will need to read in the __countries.csv__ dataset and merge together your datasets on the approporiate rows. This is because i want to create dummy variables for countries like UK and US to get a summary statistic on the pages for those countries.
<hr>

#### Conclusion
The analyis was based on the data about a website, the purpose was to analyse the data statistically to decide if the company should adapt with the new page or should stick to the old one based on the conversion of two groups (the treatment group - which is with the new page and the control group - which is witht the old group.

To reach at this goal, we performed the A/B test with z-test and logistic regression models. In A/B test, we found the p-value is higher than type error I, and therefore, we fail to reject the null hyphotesis. With the z-score, it was 1.31 which does not exceed the critical value of 1.64, so we fail again to reject the null hyphothesis. (See the conclusion on the note book).
