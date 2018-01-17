
#### Observed Trend1:
CBS News's tweets are the most positive ones among all the 5 medias'. For the passed 100 tweets, CBS's overall sentiment score is 0.37.
#### Observed Trend2:
For Fox's tweets, they are more positive during Christmas sesaon than other times.
#### Observed Trend3:
BBC's, CNN's and NYT's tweets are more neutral than CBS and Fox,because their overall sentiment score are closed to 0.


```python
import pandas as pd
import json
import tweepy
import matplotlib.pyplot as plt
import seaborn as sns
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
import numpy as np
```


```python
# Import and Initialize Sentiment Analyzer
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
```


```python
# Twitter API Keys
consumer_key = "zceXcm7pek1G8ZZTmPQmGL7uS"
consumer_secret = "djqcMgQKoKTirS617FmKvC6rKzMeE34Rv3ODVfB7rk7QmyWohY"
access_token = "548324819-njxGy0Hihw2XtZmL0cyTDD5WeqPwi8OTQY3P9aZy"
access_token_secret = "9ZRSepDKzo16yNvan4zHfMuCkq3aDCC6f0I063caSQT4S"
```


```python
# Setup Tweepy API Authentication
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)
```


```python
# Target User
target_user = ("@BBC", "@CBS", "@CNN", "@FOXTV", "@nytimes")
```


```python
sentiments_df = pd.DataFrame()
# Loop through each user
for user in target_user:

    # Variables for holding sentiments
    compound_list = []
    positive_list = []
    neutral_list = []
    negative_list = []
    data_list = []
    text_list = []

    # Loop through total 100 tweets
    for tweet in tweepy.Cursor(api.user_timeline, id=user).items(100):
        
        # Get text and date
        text = tweet.text
        date = tweet.created_at

        # Run Vader Analysis on each tweet
        compound = analyzer.polarity_scores(text)["compound"]
        pos = analyzer.polarity_scores(text)["pos"]
        neu = analyzer.polarity_scores(text)["neu"]
        neg = analyzer.polarity_scores(text)["neg"]

        # Add each value to the appropriate array
        compound_list.append(compound)
        positive_list.append(pos)
        neutral_list.append(neu)
        negative_list.append(neg)
        data_list.append(date)
        text_list.append(text)
        
        # Convert into DataFrame
    sentiments_df[user[1:] + '_Date'] = data_list
    sentiments_df[user[1:] + '_Tweet Text'] = text_list
    sentiments_df[user[1:] + '_Compound'] = compound_list
    sentiments_df[user[1:] + '_Positive'] = positive_list
    sentiments_df[user[1:] + '_Neutral'] = neutral_list
    sentiments_df[user[1:] + '_Negative'] = negative_list
    
sentiments_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BBC_Date</th>
      <th>BBC_Tweet Text</th>
      <th>BBC_Compound</th>
      <th>BBC_Positive</th>
      <th>BBC_Neutral</th>
      <th>BBC_Negative</th>
      <th>CBS_Date</th>
      <th>CBS_Tweet Text</th>
      <th>CBS_Compound</th>
      <th>CBS_Positive</th>
      <th>...</th>
      <th>FOXTV_Compound</th>
      <th>FOXTV_Positive</th>
      <th>FOXTV_Neutral</th>
      <th>FOXTV_Negative</th>
      <th>nytimes_Date</th>
      <th>nytimes_Tweet Text</th>
      <th>nytimes_Compound</th>
      <th>nytimes_Positive</th>
      <th>nytimes_Neutral</th>
      <th>nytimes_Negative</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-17 19:33:03</td>
      <td>Step inside the world of high-profile divorce ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 19:54:17</td>
      <td>2018 GRAMMY® nominee @LadyGaga is never afraid...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.1531</td>
      <td>0.000</td>
      <td>0.862</td>
      <td>0.138</td>
      <td>2018-01-17 21:16:05</td>
      <td>Cardiologists not associated with the White Ho...</td>
      <td>-0.0772</td>
      <td>0.000</td>
      <td>0.920</td>
      <td>0.080</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-17 19:08:42</td>
      <td>RT @BBCWthrWatchers: Nature's very own snowbal...</td>
      <td>0.4199</td>
      <td>0.166</td>
      <td>0.834</td>
      <td>0.000</td>
      <td>2018-01-17 19:04:12</td>
      <td>#StrangeAngel Casting News: @rupertfriend join...</td>
      <td>0.3182</td>
      <td>0.119</td>
      <td>...</td>
      <td>0.4939</td>
      <td>0.390</td>
      <td>0.610</td>
      <td>0.000</td>
      <td>2018-01-17 21:01:21</td>
      <td>One reader's reaction to North and South Korea...</td>
      <td>0.4939</td>
      <td>0.151</td>
      <td>0.849</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-17 19:07:51</td>
      <td>RT @bbccomedy: Ahead of the Bayeux Tapestry's ...</td>
      <td>0.1280</td>
      <td>0.086</td>
      <td>0.914</td>
      <td>0.000</td>
      <td>2018-01-17 14:52:00</td>
      <td>You won't want to miss performances by @eltono...</td>
      <td>0.1316</td>
      <td>0.102</td>
      <td>...</td>
      <td>0.3400</td>
      <td>0.179</td>
      <td>0.821</td>
      <td>0.000</td>
      <td>2018-01-17 21:00:11</td>
      <td>One reader's reaction to North and South Korea...</td>
      <td>0.4939</td>
      <td>0.151</td>
      <td>0.849</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-17 19:07:21</td>
      <td>RT @bbcworldservice: This pair of North Korean...</td>
      <td>0.5859</td>
      <td>0.137</td>
      <td>0.863</td>
      <td>0.000</td>
      <td>2018-01-14 22:13:29</td>
      <td>Cast your vote now and see who will be named t...</td>
      <td>0.9215</td>
      <td>0.379</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 20:46:08</td>
      <td>It’s not Apple’s fault that you feel enslaved ...</td>
      <td>-0.1134</td>
      <td>0.098</td>
      <td>0.784</td>
      <td>0.118</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-17 19:07:07</td>
      <td>RT @BBCScotland: "The more realistic they are ...</td>
      <td>-0.4201</td>
      <td>0.000</td>
      <td>0.887</td>
      <td>0.113</td>
      <td>2018-01-14 19:12:26</td>
      <td>If you continue to experience problems, please...</td>
      <td>0.3182</td>
      <td>0.242</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 20:31:48</td>
      <td>A fireball from a descending meteor lit up the...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2018-01-17 19:00:10</td>
      <td>👑 Lucy Worsley revisits key events in the live...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-14 19:12:25</td>
      <td>We apologize for the live streaming issues som...</td>
      <td>0.1027</td>
      <td>0.065</td>
      <td>...</td>
      <td>0.7384</td>
      <td>0.438</td>
      <td>0.562</td>
      <td>0.000</td>
      <td>2018-01-17 20:16:05</td>
      <td>“No longer can we turn a blind eye or deaf ear...</td>
      <td>-0.7351</td>
      <td>0.000</td>
      <td>0.772</td>
      <td>0.228</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2018-01-17 18:30:04</td>
      <td>Meet the woman who loves blood, guts &amp;amp; gor...</td>
      <td>0.6114</td>
      <td>0.307</td>
      <td>0.693</td>
      <td>0.000</td>
      <td>2018-01-14 19:11:33</td>
      <td>RT @startrekcbs: There will be lots to discuss...</td>
      <td>0.6705</td>
      <td>0.244</td>
      <td>...</td>
      <td>-0.5365</td>
      <td>0.000</td>
      <td>0.761</td>
      <td>0.239</td>
      <td>2018-01-17 20:01:31</td>
      <td>Apple plans to bring back the majority of the ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2018-01-17 17:48:46</td>
      <td>RT @bbcthree: "What do you do with your junk?"...</td>
      <td>-0.3720</td>
      <td>0.000</td>
      <td>0.896</td>
      <td>0.104</td>
      <td>2018-01-14 14:49:58</td>
      <td>Don’t miss the AFC Divisional game! Stream the...</td>
      <td>-0.2244</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 19:46:08</td>
      <td>“It’s only a matter of time before loneliness ...</td>
      <td>-0.7506</td>
      <td>0.051</td>
      <td>0.648</td>
      <td>0.301</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2018-01-17 17:30:07</td>
      <td>These coots are putting us all to shame! #GymG...</td>
      <td>-0.5255</td>
      <td>0.000</td>
      <td>0.726</td>
      <td>0.274</td>
      <td>2018-01-14 12:00:01</td>
      <td>Today's music stars will be tomorrow's legends...</td>
      <td>0.5719</td>
      <td>0.171</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 19:31:10</td>
      <td>On Tuesday, a Democrat won a Wisconsin State S...</td>
      <td>0.8176</td>
      <td>0.283</td>
      <td>0.717</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2018-01-17 17:00:07</td>
      <td>@@@\n\nEveryone uses it, but not many know its...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-13 14:32:07</td>
      <td>Don’t miss the AFC Divisional game! Stream the...</td>
      <td>-0.2244</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.6505</td>
      <td>0.000</td>
      <td>0.797</td>
      <td>0.203</td>
      <td>2018-01-17 19:21:27</td>
      <td>RT @ginakolata: Why is obesity linked to prost...</td>
      <td>-0.6597</td>
      <td>0.000</td>
      <td>0.820</td>
      <td>0.180</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2018-01-17 15:05:03</td>
      <td>🗣 Over 100 years after it was invented, Espera...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-12 21:39:27</td>
      <td>Star power makes these commercials instant cla...</td>
      <td>0.8016</td>
      <td>0.297</td>
      <td>...</td>
      <td>0.9184</td>
      <td>0.414</td>
      <td>0.586</td>
      <td>0.000</td>
      <td>2018-01-17 19:00:26</td>
      <td>The British government has appointed a ministe...</td>
      <td>-0.4215</td>
      <td>0.000</td>
      <td>0.763</td>
      <td>0.237</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2018-01-17 14:30:06</td>
      <td>Scotland has named its road gritters Sir Andy ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-12 12:00:05</td>
      <td>Four-time GRAMMY® winner @KeithUrban inspires ...</td>
      <td>0.7717</td>
      <td>0.324</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 18:45:05</td>
      <td>Moscow got only 6 minutes of sunlight during t...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2018-01-17 14:00:06</td>
      <td>Pop stars as novelists. 🎤✍\n\nOnly a select fe...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-11 20:34:24</td>
      <td>Here's why @startrekcbs needs to be your next ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4767</td>
      <td>0.279</td>
      <td>0.721</td>
      <td>0.000</td>
      <td>2018-01-17 18:30:20</td>
      <td>He leaked a photo of Rick Perry hugging a coal...</td>
      <td>-0.2023</td>
      <td>0.144</td>
      <td>0.619</td>
      <td>0.237</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2018-01-17 13:30:04</td>
      <td>Thinking of buying a car?\n\n🚙🎨 Apparently you...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-11 20:31:42</td>
      <td>RT @Elementary_CBS: Your favorite detective du...</td>
      <td>0.5550</td>
      <td>0.204</td>
      <td>...</td>
      <td>0.3400</td>
      <td>0.333</td>
      <td>0.463</td>
      <td>0.204</td>
      <td>2018-01-17 18:15:08</td>
      <td>RT @nytopinion: Nuanced conversations about co...</td>
      <td>0.2263</td>
      <td>0.079</td>
      <td>0.921</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2018-01-17 13:00:06</td>
      <td>Up until a century ago, residents of Makhunik ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-11 20:31:33</td>
      <td>RT @CodeBlackCBS: Return to Angels Memorial on...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 18:00:06</td>
      <td>Steve Bannon will be interviewed by special co...</td>
      <td>0.6908</td>
      <td>0.275</td>
      <td>0.725</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2018-01-17 12:30:01</td>
      <td>Quick-fire questions with #McMafia's @jginorto...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-11 12:00:06</td>
      <td>#TBT to when @latelateshow's @JKCorden rolled ...</td>
      <td>0.1867</td>
      <td>0.084</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 17:45:16</td>
      <td>Goldman Sachs once looked invincible. Now it’s...</td>
      <td>0.1531</td>
      <td>0.232</td>
      <td>0.580</td>
      <td>0.188</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2018-01-17 12:00:08</td>
      <td>The Bayeux Tapestry is to be displayed in UK f...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-10 17:15:18</td>
      <td>Hosts @7BOOMERESIASON &amp;amp; @DanielaRuah retur...</td>
      <td>0.8553</td>
      <td>0.344</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 17:30:32</td>
      <td>Your face may be on the wall of a museum. This...</td>
      <td>0.4019</td>
      <td>0.137</td>
      <td>0.863</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2018-01-17 11:28:12</td>
      <td>⚡️ @BTS_twt are about to appear in a @BBCR1 do...</td>
      <td>0.5461</td>
      <td>0.190</td>
      <td>0.810</td>
      <td>0.000</td>
      <td>2018-01-10 14:44:59</td>
      <td>Music's Biggest Night® just got bigger with ep...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 17:16:06</td>
      <td>RT @nytopinion: Strangely, the commander-in-ch...</td>
      <td>-0.6249</td>
      <td>0.000</td>
      <td>0.805</td>
      <td>0.195</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2018-01-17 10:34:20</td>
      <td>RT @bbcthree: Dark-skinned girls explain colou...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-09 19:30:28</td>
      <td>RT @BullCBS: Dr. #Bull's latest case could cha...</td>
      <td>0.1867</td>
      <td>0.076</td>
      <td>...</td>
      <td>0.3612</td>
      <td>0.217</td>
      <td>0.783</td>
      <td>0.000</td>
      <td>2018-01-17 17:01:05</td>
      <td>North and South Korean athletes will march und...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2018-01-17 10:31:35</td>
      <td>RT @BBCOne: Pallas's cat. It's so still… so gh...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-09 19:27:16</td>
      <td>🎶@Prince pushed the envelope and never settled...</td>
      <td>0.8360</td>
      <td>0.317</td>
      <td>...</td>
      <td>0.6360</td>
      <td>0.342</td>
      <td>0.658</td>
      <td>0.000</td>
      <td>2018-01-17 16:46:04</td>
      <td>Bill Miller, the investor who beat the S&amp;amp;P...</td>
      <td>0.5574</td>
      <td>0.195</td>
      <td>0.805</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2018-01-17 10:05:32</td>
      <td>RT @BBCSport: Manchester City Women forward Ni...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-09 18:29:39</td>
      <td>RT @AmazingRaceCBS: This race is just getting ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.2617</td>
      <td>0.204</td>
      <td>0.796</td>
      <td>0.000</td>
      <td>2018-01-17 16:31:11</td>
      <td>A plan to create a new American-backed, Kurdis...</td>
      <td>0.0000</td>
      <td>0.104</td>
      <td>0.792</td>
      <td>0.104</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2018-01-17 09:00:13</td>
      <td>'The homophobia is staggering.'\n\nFriends and...</td>
      <td>0.1027</td>
      <td>0.185</td>
      <td>0.655</td>
      <td>0.161</td>
      <td>2018-01-08 18:07:57</td>
      <td>RT @startrekcbs: #StarTrekDiscovery is back! E...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.5267</td>
      <td>0.167</td>
      <td>0.833</td>
      <td>0.000</td>
      <td>2018-01-17 16:16:09</td>
      <td>NBA Power Rankings: A team-by-team progress re...</td>
      <td>0.4215</td>
      <td>0.237</td>
      <td>0.763</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2018-01-17 08:41:10</td>
      <td>RT @BBCFOUR: 🔎  Who is your favourite actor to...</td>
      <td>0.3400</td>
      <td>0.112</td>
      <td>0.888</td>
      <td>0.000</td>
      <td>2018-01-08 14:34:48</td>
      <td>New year, new case. Watch the latest episode o...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 16:01:55</td>
      <td>In May of 2017, the Times reported that China ...</td>
      <td>-0.8271</td>
      <td>0.000</td>
      <td>0.722</td>
      <td>0.278</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2018-01-17 08:40:02</td>
      <td>Maybe we all need to find some 'monk time'?\n\...</td>
      <td>0.2023</td>
      <td>0.079</td>
      <td>0.921</td>
      <td>0.000</td>
      <td>2018-01-08 14:29:20</td>
      <td>RT @AmazingRaceCBS: Which teams are zipping th...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 15:46:05</td>
      <td>After a spate of fatal cycling crashes, offici...</td>
      <td>-0.1531</td>
      <td>0.129</td>
      <td>0.714</td>
      <td>0.156</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2018-01-17 08:39:10</td>
      <td>RT @BBCScotland: Look out the thermals, snow h...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-08 00:29:51</td>
      <td>RT @MadamSecretary: Is VP Hurst going to work ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 15:30:08</td>
      <td>RT @NYTNational: As a white supremacist prepar...</td>
      <td>0.3400</td>
      <td>0.145</td>
      <td>0.855</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2018-01-17 08:35:21</td>
      <td>RT @BBCTwo: How do you make a fruit soft drink...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-06 20:05:17</td>
      <td>RT @debrabirnbaum: .@ShawnRyanTV: To me, being...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 15:14:02</td>
      <td>Opinion: How sex trumped race https://t.co/j1o...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2018-01-17 08:34:25</td>
      <td>RT @BBCR1: ✨ Just TWO DAYS to go! ✨\n\nWatch @...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-06 19:04:08</td>
      <td>RT @LivinBiblically: “The truth is it somehow ...</td>
      <td>0.8271</td>
      <td>0.314</td>
      <td>...</td>
      <td>0.6597</td>
      <td>0.266</td>
      <td>0.645</td>
      <td>0.089</td>
      <td>2018-01-17 15:00:46</td>
      <td>Morning Briefing: Here's what you need to know...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2018-01-17 08:34:09</td>
      <td>RT @BBCWthrWatchers: Good morning Weather Watc...</td>
      <td>0.4926</td>
      <td>0.197</td>
      <td>0.803</td>
      <td>0.000</td>
      <td>2018-01-06 19:03:38</td>
      <td>RT @instinctcbs: “This is a crime show where a...</td>
      <td>-0.7717</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 14:31:07</td>
      <td>"The phenomenon of female anger has often been...</td>
      <td>-0.7906</td>
      <td>0.000</td>
      <td>0.731</td>
      <td>0.269</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2018-01-17 08:34:04</td>
      <td>RT @BBCR1: Wild, wild, wild 💫🎶\n\nWant to watc...</td>
      <td>0.0772</td>
      <td>0.058</td>
      <td>0.942</td>
      <td>0.000</td>
      <td>2018-01-06 00:37:55</td>
      <td>RT @startrekcbs: Star Trek has touched the liv...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.7269</td>
      <td>0.262</td>
      <td>0.738</td>
      <td>0.000</td>
      <td>2018-01-17 14:15:09</td>
      <td>President Trump will be encouraged to lower th...</td>
      <td>0.0772</td>
      <td>0.134</td>
      <td>0.749</td>
      <td>0.118</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2018-01-17 08:33:51</td>
      <td>RT @BBC_Teach: Teaching Shakespeare to 7-14 ye...</td>
      <td>0.2500</td>
      <td>0.095</td>
      <td>0.905</td>
      <td>0.000</td>
      <td>2018-01-06 00:30:53</td>
      <td>#StrangeAngel has found its leading man! Jack ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.2263</td>
      <td>0.000</td>
      <td>0.888</td>
      <td>0.112</td>
      <td>2018-01-17 14:00:16</td>
      <td>RT @UpshotNYT: How many ways are there to gerr...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>70</th>
      <td>2018-01-15 14:30:07</td>
      <td>It's easy to feel bogged down by bad weather a...</td>
      <td>-0.6239</td>
      <td>0.097</td>
      <td>0.669</td>
      <td>0.234</td>
      <td>2017-12-25 00:48:24</td>
      <td>Due to NFL overrun CBS is delayed 36mins in Ch...</td>
      <td>-0.2263</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 05:07:41</td>
      <td>Jo Jo White, Deadeye Shooter for Boston Celtic...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>71</th>
      <td>2018-01-15 14:02:03</td>
      <td>155 years ago, the world's first tunnel undern...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2017-12-24 20:40:42</td>
      <td>Tonight, get together and enjoy Christmas Eve ...</td>
      <td>0.7574</td>
      <td>0.289</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 05:01:11</td>
      <td>High-Fat Diet May Fuel Spread of Prostate Canc...</td>
      <td>-0.6597</td>
      <td>0.000</td>
      <td>0.645</td>
      <td>0.355</td>
    </tr>
    <tr>
      <th>72</th>
      <td>2018-01-15 13:33:03</td>
      <td>Thousands of people celebrated alternative bla...</td>
      <td>0.7845</td>
      <td>0.365</td>
      <td>0.635</td>
      <td>0.000</td>
      <td>2017-12-24 14:17:43</td>
      <td>Get ready for football! Stream NFL on CBS toda...</td>
      <td>0.7974</td>
      <td>0.273</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 04:48:04</td>
      <td>“We’re devastated.” A mother of 2 describes ho...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>73</th>
      <td>2018-01-15 13:03:25</td>
      <td>RT @BBC_Teach: The stage is set for today's #5...</td>
      <td>0.7424</td>
      <td>0.208</td>
      <td>0.792</td>
      <td>0.000</td>
      <td>2017-12-24 12:30:02</td>
      <td>All In The Family star @RobReiner gives props ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 04:33:03</td>
      <td>Understanding grief https://t.co/F1SnB7Icua</td>
      <td>-0.4939</td>
      <td>0.000</td>
      <td>0.385</td>
      <td>0.615</td>
    </tr>
    <tr>
      <th>74</th>
      <td>2018-01-15 13:01:04</td>
      <td>Martin Luther King combined radical thought, p...</td>
      <td>0.7096</td>
      <td>0.282</td>
      <td>0.718</td>
      <td>0.000</td>
      <td>2017-12-23 16:21:17</td>
      <td>Rapper @BustaRhymes pays tribute to @LLCoolJ, ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 04:18:02</td>
      <td>The majority of members of the National Parks ...</td>
      <td>-0.4588</td>
      <td>0.000</td>
      <td>0.810</td>
      <td>0.190</td>
    </tr>
    <tr>
      <th>75</th>
      <td>2018-01-15 12:33:47</td>
      <td>RT @BBCRadio2: 🎉 @achrisevans officially decla...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2017-12-23 15:00:02</td>
      <td>Oh, what fun it is to stream! Start a 1-month ...</td>
      <td>0.8236</td>
      <td>0.286</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 04:03:02</td>
      <td>Horror for 13 California siblings hidden by ve...</td>
      <td>-0.5719</td>
      <td>0.000</td>
      <td>0.764</td>
      <td>0.236</td>
    </tr>
    <tr>
      <th>76</th>
      <td>2018-01-15 12:33:01</td>
      <td>These turtles have to battle through piles of ...</td>
      <td>-0.3818</td>
      <td>0.000</td>
      <td>0.874</td>
      <td>0.126</td>
      <td>2017-12-23 12:37:04</td>
      <td>CBS stars are glowing with holiday spirit! See...</td>
      <td>0.7712</td>
      <td>0.355</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2018-01-17 03:48:04</td>
      <td>RT @FrankBruni: Lindsey Graham is no hero in t...</td>
      <td>0.5423</td>
      <td>0.197</td>
      <td>0.727</td>
      <td>0.076</td>
    </tr>
    <tr>
      <th>77</th>
      <td>2018-01-15 12:28:53</td>
      <td>RT @bbcthree: #McMafia - the real-life heists ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2017-12-23 07:24:44</td>
      <td>Spread some holiday cheer and laugh along with...</td>
      <td>0.7430</td>
      <td>0.311</td>
      <td>...</td>
      <td>0.3182</td>
      <td>0.223</td>
      <td>0.777</td>
      <td>0.000</td>
      <td>2018-01-17 03:33:05</td>
      <td>"The Bitcoin bubble may ultimately turn out to...</td>
      <td>0.3182</td>
      <td>0.218</td>
      <td>0.667</td>
      <td>0.116</td>
    </tr>
    <tr>
      <th>78</th>
      <td>2018-01-15 12:13:03</td>
      <td>Pedal power! 🚴⚡️ \n\nHow many cyclists would i...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2017-12-23 06:39:45</td>
      <td>Come home for the holidays with Lucy and Ricky...</td>
      <td>0.8687</td>
      <td>0.365</td>
      <td>...</td>
      <td>-0.1280</td>
      <td>0.000</td>
      <td>0.842</td>
      <td>0.158</td>
      <td>2018-01-17 03:18:04</td>
      <td>A photographer captures a colorful world of cr...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>79</th>
      <td>2018-01-15 12:10:56</td>
      <td>RT @bbcideas: Humans are rubbish! We get disea...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2017-12-23 01:38:37</td>
      <td>Tonight at 9/8c, gather the family for two hil...</td>
      <td>-0.1531</td>
      <td>0.104</td>
      <td>...</td>
      <td>-0.4019</td>
      <td>0.000</td>
      <td>0.597</td>
      <td>0.403</td>
      <td>2018-01-17 03:03:02</td>
      <td>CVS, the American pharmaceutical giant, has pl...</td>
      <td>-0.2960</td>
      <td>0.000</td>
      <td>0.885</td>
      <td>0.115</td>
    </tr>
    <tr>
      <th>80</th>
      <td>2018-01-15 11:56:03</td>
      <td>London's air quality is within legal limits in...</td>
      <td>0.1280</td>
      <td>0.067</td>
      <td>0.933</td>
      <td>0.000</td>
      <td>2017-12-23 00:39:52</td>
      <td>Get ready to laugh in the holidays tonight wit...</td>
      <td>0.9422</td>
      <td>0.469</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 02:48:01</td>
      <td>A lawsuit, filed by 21 state attorneys general...</td>
      <td>-0.5719</td>
      <td>0.000</td>
      <td>0.773</td>
      <td>0.227</td>
    </tr>
    <tr>
      <th>81</th>
      <td>2018-01-15 11:34:17</td>
      <td>Everybody gets down sometimes. But if it's sto...</td>
      <td>-0.1531</td>
      <td>0.000</td>
      <td>0.909</td>
      <td>0.091</td>
      <td>2017-12-22 19:35:35</td>
      <td>Tonight, ring in the holidays with two back-to...</td>
      <td>0.6588</td>
      <td>0.241</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 02:33:06</td>
      <td>Formidable tail weaponry is nearly absent in l...</td>
      <td>-0.2263</td>
      <td>0.000</td>
      <td>0.905</td>
      <td>0.095</td>
    </tr>
    <tr>
      <th>82</th>
      <td>2018-01-15 11:18:03</td>
      <td>🥛🦆 Defeated by 'youthquake' to be @OxfordWords...</td>
      <td>0.1531</td>
      <td>0.155</td>
      <td>0.714</td>
      <td>0.130</td>
      <td>2017-12-22 18:03:20</td>
      <td>Groundbreaking, daring, fearless. This is CBS ...</td>
      <td>0.6597</td>
      <td>0.278</td>
      <td>...</td>
      <td>0.4574</td>
      <td>0.299</td>
      <td>0.701</td>
      <td>0.000</td>
      <td>2018-01-17 02:17:06</td>
      <td>RT @SangerNYT: For 70 years American strategy ...</td>
      <td>-0.8074</td>
      <td>0.000</td>
      <td>0.696</td>
      <td>0.304</td>
    </tr>
    <tr>
      <th>83</th>
      <td>2018-01-15 10:37:22</td>
      <td>⛸❄️ Synchronised ice skating is NOT as easy as...</td>
      <td>0.4404</td>
      <td>0.209</td>
      <td>0.791</td>
      <td>0.000</td>
      <td>2017-12-22 14:04:52</td>
      <td>2017 Honoree @LLCoolJ is the first hip-hop art...</td>
      <td>0.5255</td>
      <td>0.159</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 02:03:03</td>
      <td>Evening Briefing: Here's what you need to know...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>84</th>
      <td>2018-01-15 10:34:23</td>
      <td>RT @BBCArchive: Cyrille Regis making football ...</td>
      <td>0.4927</td>
      <td>0.210</td>
      <td>0.790</td>
      <td>0.000</td>
      <td>2017-12-21 12:04:04</td>
      <td>She's at it again! Tomorrow, relive Lucy's hil...</td>
      <td>0.8718</td>
      <td>0.368</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 01:48:05</td>
      <td>YouTube said it altered the threshold for whic...</td>
      <td>0.3818</td>
      <td>0.115</td>
      <td>0.885</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>85</th>
      <td>2018-01-15 10:30:44</td>
      <td>RT @BBCOne: How to feel better on #BlueMonday:...</td>
      <td>0.4404</td>
      <td>0.146</td>
      <td>0.854</td>
      <td>0.000</td>
      <td>2017-12-21 01:04:27</td>
      <td>RT @survivorcbs: 6 heroes, 6 healers, 6 hustle...</td>
      <td>0.6597</td>
      <td>0.213</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 01:33:04</td>
      <td>A baby who had experimental surgery while stil...</td>
      <td>-0.6124</td>
      <td>0.000</td>
      <td>0.783</td>
      <td>0.217</td>
    </tr>
    <tr>
      <th>86</th>
      <td>2018-01-15 09:00:05</td>
      <td>💡 From wifi to drones, here are five Nikola Te...</td>
      <td>0.4215</td>
      <td>0.167</td>
      <td>0.833</td>
      <td>0.000</td>
      <td>2017-12-20 23:00:04</td>
      <td>Watch @kelly_clarkson deliver a moving perform...</td>
      <td>0.3818</td>
      <td>0.157</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.252</td>
      <td>0.748</td>
      <td>0.000</td>
      <td>2018-01-17 01:18:01</td>
      <td>RT @NYTNational: The California home where 13 ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>87</th>
      <td>2018-01-15 08:44:20</td>
      <td>RT @CBeebiesHQ: Today is #BlueMonday - the mos...</td>
      <td>-0.0240</td>
      <td>0.105</td>
      <td>0.787</td>
      <td>0.108</td>
      <td>2017-12-20 21:08:10</td>
      <td>In two days, celebrate the holidays with the g...</td>
      <td>0.7430</td>
      <td>0.249</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.252</td>
      <td>0.748</td>
      <td>0.000</td>
      <td>2018-01-17 01:03:07</td>
      <td>Philip Roth shares his thoughts on President T...</td>
      <td>0.2960</td>
      <td>0.167</td>
      <td>0.833</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>88</th>
      <td>2018-01-15 08:44:03</td>
      <td>RT @BBCBreakfast: Could you write a 500 word s...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2017-12-20 20:10:15</td>
      <td>2017 Honoree @LLCoolJ salutes James Brown for ...</td>
      <td>0.7222</td>
      <td>0.281</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 00:47:05</td>
      <td>RT @MarkMazzettiNYT: Last May, the NYT reporte...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>89</th>
      <td>2018-01-15 08:40:40</td>
      <td>RT @BBCEarth: It seems the return of the wolve...</td>
      <td>0.5574</td>
      <td>0.159</td>
      <td>0.841</td>
      <td>0.000</td>
      <td>2017-12-20 19:31:47</td>
      <td>Singer-songwriter @JoshGroban delivers a power...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 00:33:02</td>
      <td>Mathilde Krim, who has died at 91, crusaded ag...</td>
      <td>-0.5574</td>
      <td>0.000</td>
      <td>0.841</td>
      <td>0.159</td>
    </tr>
    <tr>
      <th>90</th>
      <td>2018-01-15 08:40:02</td>
      <td>Every parent needs to watch this! \n\n📱👨‍👩‍👧‍👦...</td>
      <td>0.4926</td>
      <td>0.144</td>
      <td>0.856</td>
      <td>0.000</td>
      <td>2017-12-20 15:30:01</td>
      <td>Do you know all the nominees for Album Of The ...</td>
      <td>0.5423</td>
      <td>0.143</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 00:18:03</td>
      <td>RT @nytvideo: There is no singular experience ...</td>
      <td>0.2500</td>
      <td>0.126</td>
      <td>0.787</td>
      <td>0.087</td>
    </tr>
    <tr>
      <th>91</th>
      <td>2018-01-15 08:38:25</td>
      <td>RT @BBCiPlayer: 💪 #agynessdeyn and @mrjimsturg...</td>
      <td>0.0772</td>
      <td>0.080</td>
      <td>0.920</td>
      <td>0.000</td>
      <td>2017-12-20 01:01:19</td>
      <td>RT @survivorcbs: This finale is going to be un...</td>
      <td>0.1139</td>
      <td>0.059</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-17 00:07:06</td>
      <td>Bill Miller, the investor who beat the S&amp;amp;P...</td>
      <td>0.5574</td>
      <td>0.195</td>
      <td>0.805</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>92</th>
      <td>2018-01-15 08:20:04</td>
      <td>Brain injury doesn't always result in an undes...</td>
      <td>-0.8481</td>
      <td>0.000</td>
      <td>0.635</td>
      <td>0.365</td>
      <td>2017-12-20 00:14:34</td>
      <td>This Friday, gather around the tree and celebr...</td>
      <td>0.7644</td>
      <td>0.292</td>
      <td>...</td>
      <td>0.4574</td>
      <td>0.299</td>
      <td>0.701</td>
      <td>0.000</td>
      <td>2018-01-16 23:59:08</td>
      <td>Marc Stein often knows what's happening in the...</td>
      <td>0.2960</td>
      <td>0.091</td>
      <td>0.909</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>93</th>
      <td>2018-01-15 08:01:01</td>
      <td>Sunday vibes on a #MondayMorning... 😴\n\n#BigC...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2017-12-19 21:12:50</td>
      <td>Kevin and Vanessa took a trip back in time on ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.231</td>
      <td>0.769</td>
      <td>0.000</td>
      <td>2018-01-16 23:55:03</td>
      <td>NBA Power Rankings: A team-by-team progress re...</td>
      <td>0.4215</td>
      <td>0.237</td>
      <td>0.763</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>94</th>
      <td>2018-01-15 07:38:04</td>
      <td>👑🖼 King of the collectors. \n\nA new exhibitio...</td>
      <td>0.4215</td>
      <td>0.135</td>
      <td>0.865</td>
      <td>0.000</td>
      <td>2017-12-19 21:00:03</td>
      <td>Find out how to watch A Home for the Holidays ...</td>
      <td>0.3818</td>
      <td>0.148</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.252</td>
      <td>0.748</td>
      <td>0.000</td>
      <td>2018-01-16 23:48:05</td>
      <td>Evening Briefing: Here's what you need to know...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>95</th>
      <td>2018-01-14 20:15:03</td>
      <td>Britain is the only place in the world with a ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>2017-12-19 15:30:04</td>
      <td>In one week, celebrate a night of legends when...</td>
      <td>0.5719</td>
      <td>0.209</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-16 23:33:05</td>
      <td>RT @adamgoldmanNYT: SCOOP: Ex-C.I.A. Officer S...</td>
      <td>-0.3954</td>
      <td>0.135</td>
      <td>0.577</td>
      <td>0.288</td>
    </tr>
    <tr>
      <th>96</th>
      <td>2018-01-14 20:00:09</td>
      <td>Long nights, snowshoes and a broken down boat....</td>
      <td>-0.0790</td>
      <td>0.128</td>
      <td>0.731</td>
      <td>0.142</td>
      <td>2017-12-19 14:42:58</td>
      <td>Kick off The 60th Annual #GRAMMYs with @NancyO...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.231</td>
      <td>0.769</td>
      <td>0.000</td>
      <td>2018-01-16 23:25:44</td>
      <td>Breaking News: A former CIA agent was arrested...</td>
      <td>-0.6124</td>
      <td>0.000</td>
      <td>0.783</td>
      <td>0.217</td>
    </tr>
    <tr>
      <th>97</th>
      <td>2018-01-14 19:30:08</td>
      <td>Ten times pop culture romanticised sexual hara...</td>
      <td>-0.2023</td>
      <td>0.141</td>
      <td>0.677</td>
      <td>0.182</td>
      <td>2017-12-19 00:39:07</td>
      <td>Country crooner Luke Bryan (@LukeBryanOnline) ...</td>
      <td>0.6808</td>
      <td>0.286</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-16 23:03:06</td>
      <td>NYC's comptroller is calling for an end to com...</td>
      <td>-0.4767</td>
      <td>0.000</td>
      <td>0.853</td>
      <td>0.147</td>
    </tr>
    <tr>
      <th>98</th>
      <td>2018-01-14 19:00:07</td>
      <td>No sympathy from the camera operator at all. 😂...</td>
      <td>0.0772</td>
      <td>0.214</td>
      <td>0.598</td>
      <td>0.188</td>
      <td>2017-12-18 22:53:34</td>
      <td>The search is on to find the pro football play...</td>
      <td>0.8750</td>
      <td>0.357</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>0.000</td>
      <td>2018-01-16 22:48:04</td>
      <td>J.M. Coetzee won a Nobel Prize for unsparing n...</td>
      <td>0.8591</td>
      <td>0.358</td>
      <td>0.642</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>99</th>
      <td>2018-01-14 18:33:04</td>
      <td>On the 65th anniversary of the Coronation, the...</td>
      <td>0.2960</td>
      <td>0.109</td>
      <td>0.891</td>
      <td>0.000</td>
      <td>2017-12-18 21:32:25</td>
      <td>It's the greatest gift a family can give. Cele...</td>
      <td>0.8957</td>
      <td>0.495</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.231</td>
      <td>0.769</td>
      <td>0.000</td>
      <td>2018-01-16 22:33:05</td>
      <td>Satellite imagery of the California mudslides ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 30 columns</p>
</div>




```python
# Export the data in the DataFrame into a CSV file.
sentiments_df.to_csv("NewsMood.csv")
```


```python
plt_df = sentiments_df[['BBC_Compound','CBS_Compound','CNN_Compound','FOXTV_Compound','nytimes_Compound']]
plt_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BBC_Compound</th>
      <th>CBS_Compound</th>
      <th>CNN_Compound</th>
      <th>FOXTV_Compound</th>
      <th>nytimes_Compound</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>-0.4767</td>
      <td>-0.1531</td>
      <td>-0.0772</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.4199</td>
      <td>0.3182</td>
      <td>0.8402</td>
      <td>0.4939</td>
      <td>0.4939</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.1280</td>
      <td>0.1316</td>
      <td>0.5859</td>
      <td>0.3400</td>
      <td>0.4939</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.5859</td>
      <td>0.9215</td>
      <td>0.8402</td>
      <td>0.0000</td>
      <td>-0.1134</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.4201</td>
      <td>0.3182</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
    </tr>
  </tbody>
</table>
</div>




```python
colors = ['white', 'green', 'red', 'blue', 'yellow']
BBC = plt.scatter(plt_df.index, plt_df['BBC_Compound'], marker="o", facecolors=colors[0], edgecolors="black", label='BBC')
CBS = plt.scatter(plt_df.index, plt_df['CBS_Compound'], marker="o", facecolors=colors[1], edgecolors="black", label='CBS')
CNN = plt.scatter(plt_df.index, plt_df['CNN_Compound'], marker="o", facecolors=colors[2], edgecolors="black", label='CNN')
Fox = plt.scatter(plt_df.index, plt_df['FOXTV_Compound'], marker="o", facecolors=colors[3], edgecolors="black", label='Fox')
NewYorkTimes = plt.scatter(plt_df.index, plt_df['nytimes_Compound'], marker="o", facecolors=colors[4], edgecolors="black", label='New York Times')
plt.title("Sentiment Analysis of Media Tweets (01/17/2018)")
plt.xlabel("Tweets Ago")
plt.ylabel("Tweet Polarity")
plt.ylim(-1, 1)
plt.xlim(0, 100)
tick_locations = [0,20,40,60,80,100]
plt.xticks(tick_locations, ["0", "20", "40", "60", "80", "100",])
plt.legend(bbox_to_anchor=(1,1),loc=2,title='Media Sources',fontsize=10)
```




    <matplotlib.legend.Legend at 0x1039fe278>




```python
sns.set()
plt.show()
```


![png](output_10_0.png)



```python
# The second plot will be a bar plot visualizing the overall sentiments of the last 100 tweets from each organization.
# For this plot, you will again aggregate the compound sentiments analyzed by VADER.
```


```python
users = plt_df.mean().tolist()
x_axis = np.arange(len(media))
plt.bar(x_axis, media, width=1.0, color='wgrby', edgecolor = 'bk', align="edge") 
```




    <Container object of 5 artists>




```python
tick_locations = [value+0.4 for value in x_axis]
plt.xticks(tick_locations, ["BBC", "CBS", "CNN", "Fox", "NYT"])
plt.title("Overall Media Sentiment based on Twitter (01/17/2018)")
plt.ylabel("Tweet Polarity")
plt.ylim(-0.1, 0.4)
plt.xlim(0, len(x_axis)) 

for a,b in zip(x_axis,users):
    plt.text(a+0.4, b+0.01, str(round(b, 2)), ha='center', va= 'bottom',fontsize=10, fontweight='bold')
```


```python
sns.set()
plt.show()
```


![png](output_14_0.png)

