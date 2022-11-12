## Analyze A/B Test Reesults
<hr>
For this project, I will be working to understand the results of __an A/B test run by an *e-commerce website*__. The company has developed a new web page in order to try and increase the number of users who "convert," meaning the number of users who decide to pay for the company's product. My goal is to work through this notebook to help the company understand if they should implement this new page, keep the old page, or perhaps run the experiment longer to make their decision.
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
- The number of rows in the dataframe. code: df.shape
- The number of unique users. code: df.user_id.nunique()
- The porstion of converted users. code df.converted.mean()
- The number of times new_page and treatement don't line up. Code: `df[((df['group'] == 'treatment') == (df['landing_page'] == 'new_page')) == False].shape[0]`
- If the there is any missing values in the dataset. code: df.isnull().any()
