---
date: "2020-03-12"
external_link: ""
image:
  caption: 
  focal_point:
links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/asvn90
slides: 
summary: How to create a Twitter bot
tags:
- Python
- Twitter
title: Create a Twitter bot
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
---
As part of my aim to learn Python, I decided to teach myself by completing a bunch of projects created in Python, and a Twitter bot is my first project!

The purpose of this article is two-fold:

- Helping you to create a Twitter bot of your own.
- Record the process for when I return to this project in the future.

**Right! Let's get started!**

## Some background watching

I came across a video by CS Dojo [How To Create A Twitter Bot With Python](https://www.youtube.com/watch?v=W0wWwglE1Vc&t). Do check it out!

## Getting started with Twitter API
The first thing you need to do is create an account at [Twitter Developer](https://developer.twitter.com/). Make sure you are logged into the Twitter account that you want to use to create the Twitter bot. Create your account and add an app that will generate your authorisation keys that you need to connect to the Twitter API.
These keys will be used in the Python script to connect to the API.

## Connect to the Twitter API with Python
The first thing you need to do is to install `tweepy` which is the Python library that you will use to connect to the Twitter API. You can install is using the command prompt:

    pip install tweepy
    
Once that is done, you can get started with setting up your `twitter_bot.py` script.

    import tweepy

    CONSUMER_KEY = 'YOUR_CONSUMER_KEY'
    CONSUMER_SECRET = 'YOUR_CONSUMER_SECRET_KEY'
    ACCESS_KEY = 'YOUR_ACCESS_KEY'
    ACCESS_SECRET = 'YOUR_ACCESS_SECRET_KEY'

    auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
    auth.set_access_token(ACCESS_KEY, ACCESS_SECRET)
    api = tweepy.API(auth, wait_on_rate_limit=True)

You can copy the four key values from your created app in your Twitter Developer account. 

>If you plan to commit your code to an online repository, do not add your access keys to your code directly, as others will be able to see it.

The last three lines of the code sets up the API so that you can use it for all kinds of *cool* stuff. Make sure you go have a look at the [Tweepy documentation](http://docs.tweepy.org/en/latest/) to find out what the `tweepy` library can do.

## Fav and retweet!
For this project, I want to create a bot that will *favourite* a tweet and *retweet* it when your twitter account is mentioned.

### Scan for any tweets mentioning the account
First we need to collect all the tweets that mention the account. `Tweepy` enables you to collect the 20 latest tweets mentioning your account.
    
It is necessary to specify the last tweet ID that you interacted with to ensure that you don't retweet *or* favourite any tweet you already have.

We can store the ID of the last tweet that we enteracted with in a .txt file.

    def retrieve_last_seen_id(file_name):
      f_read = open(file_name, 'r')
      last_seen_id = int(f_read.read().strip())
      f_read.close()
      return last_seen_id

The function opens up the textfile and reads the tweet ID and returns it as an integer in the variable `last_seen_id`.

Next we retrieve all the tweets mentioning your twitter account after the last tweet ID specified in your textfile.
    
    mentions = api.mentions_timeline(last_seen_id, tweet_mode = 'extended')

We loop through all the tweet IDs in the `mentions` variable. The loop starts at the end of the list, which will be the oldest tweet that we haven't interact with yet.

    for mention in reversed(mentions):
        if not mention:
            return
        last_fav_tweet = mention.id
        store_last_seen_id(last_fav_tweet,FILE_NAME_FAV)
        
The tweet ID of the last interacted tweet is updated in the text file, using the function:

    def store_last_seen_id(last_seen_id,file_name):
      f_write = open(file_name, 'w')
      f_write.write(str(last_seen_id))
      f_write.close()
      return

The tweet ID stored in the `mention.id` is then *favourited* and *retweeted* with the commands:

    api.create_favorite(mention.id)
    api.retweet(mention.id)
    
## Deploying the bot
Now that you have set up the bot it needs to run continuously to check for tweets mentioning your account. This can be done by putting the function in an infinite loop:

    while True:
      fav_tweets()
      time.sleep(15)

The code `time.sleep()` pauses the `while` loop for a period specified here as 15 seconds. Make sure to add 

    import time
    
to your script to enable this functionality. 

### Using Heroku
The problem with running the bot locally on your computer is that your command prompt window will be occupied running the bot, so you won't be able to use it. 

The best solution is to deploy it on a server, and [Heroku](https://www.heroku.com/home) is a free service for just that purpose!
The best place to start is to follow Heroku's [step-by-step guide for deploying using Python](https://devcenter.heroku.com/articles/getting-started-with-python).

You will need to sign up for an account and then install the *Heroku CLI* to your computer. You can either link your Heroku account to your Github repository where your bot has been uploaded, or you can upload it to a *Heroku repository*. 

We will use the *Heroku repository*. Once you have installed the *Heroku CLI* to your computer, login using the command prompt:

```cmd
$ heroku login
```

### Create a Heroku app

The next step is to initiate a git repository on your local machine using:

```cmd
$ git init
$ cd the-name-of-your-bot
```

>You can also clone the `getting-started-with-python` repository that Heroku provides. It has all the additional documents already created that you need. All you need to do is copy those files over to your project's folder.

After a git repository has been initialised, you need to commit all your changes and then create an app on Heroku, which prepares Heroku to receive your source code:

```cmd
$ git add .
$ git commit -m "initial commit"

$ heroku create

$ git push heroku master
```

### Last few things
So the last problem is that you need to get your API authorisation keys to the Heroku repository without making it public knowledge.

Before you push your repository to Heroku, make the following changes to your code:
    
    import os
    from os import environ
    
    CONSUMER_KEY = environ['YOUR_CONSUMER_KEY']
    CONSUMER_SECRET = environ['YOUR_CONSUMER_SECRET']
    ACCESS_KEY = environ['YOUR_ACCESS_KEY']
    ACCESS_SECRET = environ['YOUR_ACCESS_SECRET']
    
The above code makes sure that you do not need to commit your Twitter API keys. After you have edited your code, you need to add your API keys to your Heroku account by going to your Heroku app, going to settings and editing the *Config Vars*.

Also, there are a few more files that you need to upload to Heroku before it will be functional. This include:

- Procfile
- Requirements.txt
- Runtime.txt

>This is only necessary if you haven't already copied them over from the repository `getting-started-with-python` that you cloned.

### Procfile
Heroku apps include a Procfile that specifies the commands that are executed by the app on startup. You need to use a Procfile to declare that this is a worker:

    worker: python your_bot_name.py
    
### Requirements.txt
The requirements file lists the versions of all the programs that your bot will use:

    certifi==2019.11.28
    chardet==3.0.4
    idna==2.9
    oauthlib==3.1.0
    PySocks==1.7.1
    requests==2.23.0
    requests-oauthlib==1.3.0
    six==1.14.0
    tweepy==3.6.0
    urllib3==1.25.8

### Runtime file
This file specifies the version of Python that Heroku needs to use. At the time of writing this, Tweepy does not support Python 3.7, so you need to specify an older version.

    python-3.6.9

