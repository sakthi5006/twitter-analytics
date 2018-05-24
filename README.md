# twitter-analytics
twitter scrapers for analysis

* for running the scripts given in the steps following libraries are used.

* `tweepy`
* `jsonpickle`

### for downloading required libraries
```bash
pip3 install -r requirements.txt
```

* for machines with only a single python installed we can use `pip` directly instead of `pip3`

## for step one

### format for the step one

|serial_number | screen_name| user_id| tweet_id| retweet_count| date| tweet|
| ----         |  ----------| -------| --------| --------     | --- | ---- |
|              |            |        |         |              |     |      |

```python3
python3 extract_tweets.py input
```
* This will download the tweets in `problem_large.txt` file.
* Then run the following command.

```python3
python3 txt_to_csv.py
```
* This will produce `output.csv` file.

## for step two

Step 2:  from the step 1 output observe( 5th column of the table) i.e number of re tweets obtained for each  tweet .
If number of re tweets obtained for the given tweet is  0 then discard the tweet other wise print the tweet in the above format.

Output :  print only the tweets which got re tweets and discard the tweets with no re tweets

* The step to do is copy `output.csv` into this `step_2` folder.

```python3
python3 remove_zero_tweets.py
```
* The output will be a `step_two_output.csv` file
* This will contain the tweets with more than zero retweets.

## for step three

Step 3: Find out number of users who has been tweeted those tweets in step 2, because one user may post multiple tweets.

Input: output of step 2

Output:

|serial_number | user_name @mention | user_id| #tweets (no of tweets posted by user)|
| ----         |  ----------        | -------| --------                             |
|              |                    |        |                                      |

The folder `step_3` should contain `step_two_output.csv`

```bash
mkdir step_3
cd step_3
cp ~/twitter-analytics/step2/step_two_output.csv .
```
run the following python code

```python3
python3 unique_user_identifier.py
```

## for step four
All the users who are there in the output of step 3 are not influential users, to find out
Influential users from the above table, find out no of retweets obtained for each user
and calculate weight or user rank.

Input: Step 2 output

output format: 

|serial_number | user_name @mention | user_id| #tweets (no of tweets posted by user)| # retweets | log(#retweets)|
| ----         |  ----------        | -------| --------                             | ----       | -----         |
|              |                    |        |                                      |            |               |


```bash
mkdir step_4
cd step_4
cp ~/twitter-analytics/step2/step_two_output.csv .
cp ~/twitter-analytics/step3/step_three_output.csv .
```

```python3
python3 influential_users.py
```

## for step five
from the above table from step four, we've calculated weights of each user, from that pick out those users,
whose weight > 1.5

Input: step_four_output.csv

Output format:


|serial_number | user_name @mention | user_id| #tweets (no of tweets posted by user)| # retweets | weights > 1.5 |
| ----         |  ----------        | -------| --------                             | ----       | -----         |
|              |                    |        |                                      |            |               |


```bash
mkdir step_5
cd step_5
cp ~/twitter-analytics/step4/step_four_output.csv .
```

```python3
python3 weighted_users_seperator.py
```

## for step six
In step five, 2nd column of the table, count the number of users, # users are called as `Influential Users`


## for step seven
For Influential users, calculate global influential score for each user.

$$

Influential score formula = \frac{no of retweets}{no of tweets}

$$


|serial_number | user_name @mention | user_id| #tweets (no of tweets posted by user)| # retweets | global Inf score|
| ----         |  ----------        | -------| --------                             | ----       | -----           |
|              |                    |        |                                      |            |                 |

### Running the scraper
```python3
python3 main_scraper_twitter.py input
```

* this will download the tweets based on the input file
* we can change the no of tweets that can be scraped.
* Just change the maxqueries in the `main_scraper_twitter.py`
* The `main_scraper_twitter.py` uses `tweepy` and `jsonpickle`
* This will produce the `large.txt` file

### for obtaining the no of tweet count
```bash
python3 no_of_tweets_column.py large.txt
```

* This will produce `top_4.txt` and `added_column.txt`
* The `top_4.txt` has the top 4 persons who has tweeted most tweeted most
* The `added_column.txt` has the list of all the persons who has tweeted
  in the order of their tweet count.


### Problem 1
* scraping large number of users from twitter
* who tweeted about a particular topic in a specified interval of time

### Problem 2
* arranging the users in the order of the number of tweets they made.

### Problem 3
* finding the users who tweeted the most in a specified interval of time.
* giving their tweets

```bash
python3 third_problem_output.py large.txt
```

* this will produce a txt file `third_problem.txt`
