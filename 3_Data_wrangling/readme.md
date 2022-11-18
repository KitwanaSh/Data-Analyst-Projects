## Data Wrangling Project
This project is based on the second phase of data analysis process. The datasets that helped me to work on this project is about the __#WeRateDog data__.

### Tool used
If you are a data analyst student or professional in experience and you would like to go through similar process as i did in this project, you will need have:
- Anaconda (jupyter note book) installed in your pc
- Import usefull libraries like:
    - `Pandas`
    - `Numpy`
    - `tweepy`
    - `json`
    - `timeit`
    - `Matplotlib`
    - `Seaborn`
- A twitter developer account to help you aquire the data using the `twitter api`

### 1. Data Wrangling Efforts
In order for me to successfully get this project done, I followed the three steps that are required for the data wrangling. They are:
- Data Gathering;
- Data Assessment;
- Data Cleaning.

For the data gathering, I acquired the data from three (3) different sources and ended having files with different format; one in `CSV`, another one in `TSV`, and the last one in `.TXT` extension, it was a __JSON file__. I eventually converted the JSON file into a csv so I can easily manipulate all the files together. <br>
My second dataset to acquire was a tsv, which i got the _image-prediction.tsv_ file using the `requests.get()` and `file.write()` funtion. Here is the code:

```
import requests
url = 'https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv'
response = requests.get(url)

with open('image-prediction.tsv', mode='wb') as file:
    file.write(response.content)
```
The gathering process required that one of the data needed to be downloaded using the tweeter api. Unfortunately, it could not be possible for me to do this and meet that requirement. But that’s okay for now, we were given a chance to go on without using the api just in case there could be a problem (like my case, I was unable to have a tweeter developer account on time …). But i have the account now and i am using the same techniques thaught in class to get the data from twitter using the `tweepy library`. So, I have donwload the file now usinf the api as my account is now approved as a twitter developer but you can skip this step and imediately read the json twitter dataset (`tweet_json.txt`).<br>
In case you are curious about how to aquire the dataset from twitter using its api, you can create an account (_which is different from the ordinary twitter account_) on the [twitter developer website here]('https://developer.twitter.com/en/support/twitter-api/developer-account') and you will be asked to fill in their requirement in order for you to have an account. Note the approval is based on the information you will provide to them showing the reason why you want to use the twitter dataset and i might take up to 3-5 working days for them to give you the approval or any other type of feedback.<br>
Anyways, the code below is the one i used to get the __WeRateDog__ dataset from twitter. In the code below, there this is _HIDDEN_, put the personal information based on your twitter developper account.


```
# Query Twitter API for each tweet in the Twitter archive and save JSON in a text file
# These are hidden to comply with Twitter's API terms and conditions
consumer_key = 'HIDDEN'
consumer_secret = 'HIDDEN'
access_token = 'HIDDEN'
access_secret = 'HIDDEN'

auth = OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_secret)

api = tweepy.API(auth, wait_on_rate_limit=True)

tweet_ids = df_1.tweet_id.values
len(tweet_ids)

# Query Twitter's API for JSON data for each tweet ID in the Twitter archive
count = 0
fails_dict = {}
start = timer()
# Save each tweet's returned JSON as a new line in a .txt file
with open('tweet_json.txt', 'w') as outfile:
    # This loop will likely take 20-30 minutes to run because of Twitter's rate limit
    for tweet_id in tweet_ids:
        count += 1
        print(str(count) + ": " + str(tweet_id))
        try:
            tweet = api.get_status(tweet_id, tweet_mode='extended')
            print("Success")
            json.dump(tweet._json, outfile)
            outfile.write('\n')
        except tweepy.TweepError as e:
            print("Fail")
            fails_dict[tweet_id] = e
            pass
end = timer()
print(end - start)
print(fails_dict)
```

<br>
After, the gathering, i dealt with the assessment which was just about looking at the raw data in different prospectives, by observing the data _in visual form_ and also in _programmatical form_. To assess the data programmatically, I used a various of Pandas methods. I order to prepare for the next step, I had to document the issues in found from the data during this step. Some of the methods used during the assessment were (`df.head()`, `df.sample(20)`, `df.info()`, `df.name.value_counts()`, `df.columns`, `df.column_name.nunique()`, `df.duplicated()`, …).

Then the last step of data wrangling was to clean the data based on the issues I found during the assessment. Cleaning is the hardest and longest step in the wrangling step and it didn’t deceive. I went through a lot along this process. Most of the times, I went back to clean more data during this process and even after the process, during the analtsis and visualization, I could go back and redo the cleaning after observing a mistake. All this because the wrangling step is __iterative__.

After doing all the steps above, I stored the cleaned data to a new `csv` using the `to_csv` pandas method and also using the `to_sql` method. Then, I eventually gave a little conclusion from the cleaned data.
<br>

### 2. Act
This part is included in the project to share the insight found during the whole wrangling even thought it is not part of wranling.

a. Showing the relationship between numeric columns in the heatmap, histogram and Scatter plot
![alt-image](image.png)

##### Insights
- The only relationship we can mention here, is the strong positive correlation between the retweets count & favorite count. and that makes sense.
- And we can say that the more images the tweet has, the more likely to get retweets & favorites

b. The distribution of numeric attributes
![alt-image2](image.png)

##### Insights
- Only the p1_confident is left skewed, that means it is where the most values occurred at.
- The Rest of numeric columns are right skewed

c. The ralationships between retweet and favorite count
![alt-mage](image.png)

##### Insights
- Most of tweets had retweets less than 10,000 and favorites up to 32,000.
- We have a few tweets that got much more favorites & retweets.

d. Proportion of success in dogs
![alt-dig](image.png)

##### Insights
- p2_algorithm is the most successful algorithm
- p3_algorithm is the least successful algorithm
- all algorithms tend to roll around the similar portion (only a little difference).

e. Most common dog names
![iamge-comong-dogs](image.png)

#### Insights
- The top 10 most common names are Lucy, Charlie, Oliver, Cooper, Penny, Tucker, Sadie, Lola, Winston and Daisy
- Lucy and Charly have the same number of common names
- Lucy, charlie Oliver and Cooper are the top 10 names to be common in more than 9 times.

After this insight, you are able to give a conclusion but we are not going to do it here.