---
layout: post
title: Twitter-bot 
date: 2020-05-05 01:29
summary: How to build a simple twitter-bot in python and deploy it on Heroku to keep the program running even when your computer is sleeping or turned off.
categories: jekyll pixyll
---

A Twitter bot allows us to control a Twitter account via the Twitter API. We could make our bot to tweet, retweet, like or follow from our account, and the best part would be that it does all these taks autonomously. 

## Pre-requisites:
If you have some basic knowledge about Git and Python, the tutorial would be really easy to follow. But don't worry if you have just started the journey, every doubt is just one Google search away.
To start off things, you will need-
* Python programming environment
* A Twitter account

## Setting up Twitter and tweepy
* Create a Twitter account. 
> I will advise you to give the bot its own account than your primary account as we will be using it for testing. 

* After creating the account go to [Twitter's developer portal](https://developer.twitter.com/en/portal/dashboard) to create a new app while being logged in from bot’s account. While creating the twitter app, If asked for app’s website you could fill in your GitHub URL where your project might be.

* You should get your Consumer Key, Consumer Secret, Access Token, and Access Token Secret after creating the app.

* Create a directory for your project and move into it. Then create a <span class="red">tweetbot.py</span> file inside the directory.
```python
foo@bar:~$ mkdir tweetbot
foo@bar:~$ cd tweetbot
foo@bar:~/tweetbot$ nano tweetbot.py
``` 

* Install <span class="red">tweepy</span> in your python environment which is required to access Twitter's API and import it into tweetbot.py file.
```python
foo@bar:~/tweetbot$ pip install tweepy
import tweepy
```

* Create another file with name <span class="red">credentials.py</span> to store Twitter Consumer Key, Consumer Secret, Access Token, and Access Token Secret.
```python
foo@bar:~/tweetbot$ nano credentials.py 
consumer_key = 'your_consumer_key'
consumer_secret = 'your_consumer_secret'
access_token = 'your_access_token'
access_token_secret = 'your_access_token_secret'
```

> <span class="red">Note:</span> We are storing them in a separate Python file than the main one because anyone who has access to these strings can use our Twitter account, so we don’t want to share these or make them public. Also, don’t commit these to a Git repository, instead, you can add them to .gitignore.

* Move into your tweetbot.py file and import the credentials to use. We’ll also add some lines to interact with the credential variables via the Tweepy library.

```python
import tweepy
# Import our Twitter credentials from credentials.py
from credentials import *

# Access and authorize our Twitter credentials from credentials.py
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)

```

## Content for tweets & defining the functions
* Create a text file <span class="red">content.txt</span> to get the content for the tweets. You could use anything that you like. I’m using the text from a book that I got from [Project Gutenberg ](https://www.gutenberg.org/), which provides free eBooks (mostly in the public domain) to readers. 

* We have to create some variables and add relevant functions to them.

```python
import tweepy
from credentials import *

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)

# Open text file content.txt (or your chosen file) for reading
my_file = open('content.txt', 'r')

# Read lines one by one from my_file and assign to file_lines variable
file_lines = my_file.readlines()

# Close file
my_file.close()
```

* Now that we have our lines stored in a variable, we are ready to tweet from our Twitter-bot account. To automate our tweets in a time-based way we will import the module <span class="red">time</span>, we will only use <span class="red">sleep</span> function here, so we import that only.

```python
# Add all import statements at top of the file
import tweepy
from time import sleep
from credentials import *
…
```

* For automation, we require a for loop to iterate over the lines from our content.txt file saved in <span class="red">file_lines</span> variable. To see the output, we will print it out.

```python
import tweepy
from time import sleep
from credentials import *

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)

my_file=open('content.txt','r')
file_lines=my_file.readlines()
my_file.close()

# Create a for loop to iterate over file_lines
for line in file_lines:
    print(line)
```

* On running the program we will see that it’s printing out the lines into the terminal window from our text file. We want every line as our new tweet and so we’ll use the tweepy function <span class="red">api.update_status( )</span>. This is used to update the authenticated user’s status, but will only update if the status is either: 1) not a duplicate, or 2) 140 characters or less. Add the function by passing the line variable into it.

```python
...
for line in file_lines:
    print(line)
    api.update_status(line)
```

## Final touches
* We have our working program with us, running it at this point won’t get us far and we'll get some error. We still need to handle blank lines in our text. For doing that we will add an <span class="red">if</span> statement and also <span class="red">sleep( )</span> to ensure that these tweets don’t go out all at once.

```python
...
for line in file_lines:
  print(line)

# Add if statement to ensure that blank lines are skipped
  if line != '\n':
      api.update_status(line)

# Add an else statement with a pass to conclude the conditional statement
  else:
      pass

# Add sleep method to space tweets by 5 seconds each
  sleep(5)
```

* We still need to handle one error for duplicate status, it shouldn’t post the same tweets everytime from the start. To do that, we’ll add a <span class="red">try ... except</span> block to our code, and also have the console print out the reason for the error.

```python
...
for line in file_lines:
# Add try ... except block to catch and output errors
    try:
        print(line)
        if line != '\n':
            api.update_status(line)
        else:
            pass
    except tweepy.TweepError as e:
        print(e.reason)
    sleep(5)
```

Now, we have our full working twitter bot. On running the program, you could see tweets updating from your account. You might want to increase the sleep time to <span class="red">sleep(3600)</span> to tweet every one hour, just be aware of twitter’s API policies.

## Deploying on Heroku

Because Twitterbots do ongoing and automated tasks, you may want to keep the program running even when your computer is sleeping or turned off. With a server, you can keep your program running as long as you would like. So, now we will put our bot on Heroku.

* Firstly, push the code to a Git repository and don’t forget to add <span class="red">credentials.py</span> to <span class="red">.gitignore</span> 

* Log into your Heroku account or [create one](https://signup.heroku.com/) if you don’t have. Heroku enables developers to build, run, and operate applications entirely in the cloud and you can create up to 5 apps for free.

* Install the Heroku [command-line interface](https://devcenter.heroku.com/articles/heroku-cli). Once, its installed, initialize your repo with Heroku and it will generate a random URL.

```python
$ heroku create
```
This operation will set up an app and also give a remote to our Git repo called heroku

* We need to create some files for telling Heroku what to do once it gets our code. Create three files <span class="red">Procfile</span>, <span class="red">requirements.txt</span> and <span class="red">server.py</span>

### server.py:
We need to have some kind of web server that will listen on the Heroku PORT, otherwise, Heroku will think our app isn't working and it will be in the 'crashed' state. To satisfy this Heroku requirement, we can run a really simple Flask web server like this:

```python
from os import environ
from flask import Flask

app = Flask(__name__)
app.run(host= '0.0.0.0', port=environ.get('PORT'))
```

### requirements.txt:
This will list our dependencies, and it will be detected by Heroku and tell it that we are running a python app. Our file will look like this:

```python
tweepy==3.8.0
django-heroku
Flask
```

> To check the correct value for tweepy, you can type this in your terminal:
```python
$ pip freeze | grep tweepy 
```

### Procfile:
Put this in your Procfile:

```python
web: python server.py
worker: python tweetbot.py
```

The first command helps us to connect to the server and the second command will tell Heroku to do the equivalent of what you do in your terminal. 

Since Heroku uses Git repo for deployment, so constants from .gitignore won’t work. The last thing we will have to do is to set our <span class="red">environment variables</span> in Heroku and then use <span class="red">os.environ</span> to get them.

* Go to the [Heroku portal](https://dashboard.heroku.com/apps) and in your app settings, you have to add our 4 credentials variables one by one in Config Vars.

> Also check in the overview tab in Heroku that the dynos are in on state, if not, turn them on.

* Now, in your tweetbot.py file add these lines at the top:

```python
from os import environ
CONSUMER_KEY = environ['CONSUMER_KEY']
CONSUMER_SECRET = environ['CONSUMER_SECRET']
ACCESS_KEY = environ['ACCESS_KEY']
ACCESS_SECRET = environ['ACCESS_SECRET']
```

* Finally, we just need to commit our code and push it to heroku

```python
$ git commit -m “message”
$ git push heroku master
```

> Here I am deploying from the master branch, you could also deploy from a different branch by:
```python
$ git push heroku my_branch_name:master
```

Our app will be up and running. You can check activity logs from Heroku or else your Twitterbot account for seeing the status updates.
This bot only tweets from your account. We could also create one for retweeting, favoriting and following. If you want to do that, you just need to change the code in your tweetbot.py file. Checkout [this](https://drivecrux.github.io) article if you want to do that.

You can also checkout the [source code](https://github.com/drivecrux/tweetbot.git), in case of any doubt.











 

