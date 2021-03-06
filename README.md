# WinBot
## Required
Make sure you have
[Python](https://www.python.org/downloads/)
and [Twython](https://twython.readthedocs.io/en/latest/usage/install.html) installed on your machine.


### Connection with Twitter
You will need an app on twitter. You can create it right here: [https://apps.twitter.com/](https://apps.twitter.com/)    
Copy **app_key**, **app_secret**, **oauth_token** and the **oauth_token_secret** into the `winbot.py`-file.



## Run it
`$ python winbot.py`



## Settings
Have a look at the `tell settings`-function and tweak some settings to make this bot fit your needs:
```
def tell_settings():
    settings = {'search_for' : 120,
                'cycles': 2,
                'sleep' : 135,
                'interaction_limit' : 4,
                'max-follow' : 2000,
                'max_mentioned_follow' : 3,
                'search_query' : 'rt2win OR retweet to win -filter:retweets AND -filter:replies'
    }
    return settings
```

### search_for
This value defines for how many tweets the bot will search for in one cycle. Twitters API provides at max. 150 tweets at a time when searching with a certain query. Because we filter for already retweetet tweets, blacklisted users/strings, retweets and replies, we won’t get as many tweets to retweet as we searched for. But that’s okay, because this bot works in cycles and searches for tweets over and over again – as often as you want.  
Choose how often the bot will repeat the search-and-retweet-process by changing the amount of **cycles**.

### cycles
In one cycle, the bot will searches for tweets to retweet, then retweets these tweets, follows their authors and logs some actions. If you want this bot to do that again and again, just increase the number of cycles.

### sleep
Because Twitter does not provide us with endless data, there are some limits for interactions through twitters API. About 150-180 Interactions in 15 minutes (900seconds) are allowed. The value of **sleep** represents the amount of seconds the bot will wait every time the **interaction_limit** is reached.

### interaction_limit
Every interaction with twitter is counted. Whenever the **interaction_limit** is reached, the bot will pause in order to prevent getting blocked by twitter because of exceeding limits.  
Play with this value, *if you're feeling lucky*.  
Reading the [rules for twitters limits](https://support.twitter.com/articles/355430) could help, though.

### max_follow
This value represents the max. amount of users this bot will follow at a time. If reached, the bot will unfollow the user you followed for the longest time. The default for this value is 2000. (Twitter will block users, who follow more than about 4.000 users.)
Unfollowing clearly costs time, but it is essential for this bot to run on the long term without being blocked by twitter.

### max_mentioned_follow
Some twitter contests want you to follow not only one user to be part of it. They mention other users you need to follow in order to participate. Because of that, this bot finds metioned users in tweets and follows them. It’s unlikely that you need to follow more than two or three users for a single contest, therefore you can set the limit for following mentioned users (per tweet) with **max_mentioned_follow**. If set to zero, no mentioned user will be followed.

### search_query
This string is the actual search query. You can manipulate it and explore, what string works best for you. You can find a guide for building these querys right [here](https://dev.twitter.com/rest/public/search).

## Files
### log.txt
All retweets are logged here. Just so that you can keep an eye on the progress. Also the bot compares the results of a search with all tweets from this log-file. Because of that, this bot won't try to retweet a tweet twice, which saves a lot of time.

### follows.txt
This is where every user you followed is saved. It is fundamental to the unfollow-process (explained below).

### blacklisted-users.txt
Here you can manually save names of users (seperated by linebreak) you don't want to interact with. Unfortunately it's not possible to teach a bot to avoid interaction with blocked users. On the other hand the blacklist can be copied and used with multible other bots.
The blacklist is useful, because every unwanted retweet costs time.  

In the new folder
[filled-blacklist](https://github.com/jflessau/winbot-twitter-bot/tree/master/filled-blacklist)
you can find an alternative blacklisted-users.txt with the most annoying BotSpotterBots in it. I'll give my best to keep it up to date.

In case you got some more names, don’t hesitate to contribute them!

### blacklisted-strings.txt
As required by [Darkcast](https://github.com/Darkcast?tab=repositories) you can now save strings (e.g. hashtags) in this file (seperated by linebreak). The bot will ignore tweets with those strings.  
I’ve added a filled version in the [filled-blacklist-folder](https://github.com/jflessau/winbot-twitter-bot/tree/master/filled-blacklist).



## Additional info

### Why is this bot unfollowing users?
Following to many users at a time can get your bot blocked. Because of that, it also unfollows users ([fifo-princliple](https://en.wikipedia.org/wiki/FIFO)).

### It’s kinda time consuming - what can I do about that?
Because of twitters limits, theres not much you can do about that. Let this bot run on a raspberry or arduino, control it with [cron](https://en.wikipedia.org/wiki/Cron) and wait for some prices :)
