# Discord <-> Ifttt Webhooks

Just my little collection of [Ifttt.com](https://ifttt.com/my_applets) apps which are linked to my [Discord webhooks](https://support.discord.com/hc/en-us/articles/228383668-Webhooks-gebruiken).


* [Discord <-> Ifttt Webhooks](#discord---ifttt-webhooks)
  * [Common mistakes I (_and others_) often run into](#common-mistakes-i-and-others-often-run-into)
  * [Useful websites to help you with regex & Discord Embeds](#useful-websites-to-help-you-with-regex--discord-embeds)
  * [Online formatters/validators](#online-formattersvalidators)
  * [How a typical webhook must look like](#how-a-typical-webhook-must-look-like)
  * [What does `Action failure message: Rate limited by the remote server` mean?](#what-does-action-failure-message-rate-limited-by-the-remote-server-mean)
  * [Debug possible errors](#debug-possible-errors)
    * [Advance YouTube Upload finished announce feed](#advance-youtube-upload-finished-announce-feed)
    * [Android App Updates](#android-app-updates)
    * [Reddit Game Findings (_works basically with every Giveaway/Gift Subreddit_)](#reddit-game-findings-works-basically-with-every-giveawaygift-subreddit)
    * [Thank user for the follow on Twitter](#thank-user-for-the-follow-on-twitter)
    * [Twitch went live Feed](#twitch-went-live-feed)
    * [Twitch with viewer count & embed preview](#twitch-with-viewer-count--embed-preview)
    * [Twitter Basic Feed](#twitter-basic-feed)
    * [Twitter Advance Feed](#twitter-advance-feed)
    * [Twitter Advance Feed with Embed](#twitter-advance-feed-with-embed)
    * [Instagram (complex)](#instagram-complex)
    * [Instagram (simple)](#instagram-simple)
    * [Nitter (Twitter) Tweet vi role-id](#nitter-twitter-tweet-vi-role-id)
    * [RSS Feed (Basic)](#rss-feed-basic)
    * [RSS (Advance)](#rss-advance)
  * [Old and unused stuff](#old-and-unused-stuff)
    * [Pizza Delivery (_I do not use it anymore since YAGPDB has a reminder function_)](#pizza-delivery-i-do-not-use-it-anymore-since-yagpdb-has-a-reminder-function)
    * [Tumblr](#tumblr)
    * [Facebook](#facebook)
    * [SoundClood](#soundclood)
    * [GitHub Webhook](#github-webhook)
    * [Yet another basic Twitter feed](#yet-another-basic-twitter-feed)
    * [NASA - Image of the Day](#nasa---image-of-the-day)
    * [Basic RSS](#basic-rss)
    * [My first webhook ever (Twitter)](#my-first-webhook-ever-twitter)
  * [Unfinished](#unfinished)
    * [Wind & Weather](#wind--weather)


## Common mistakes I (_and others_) often run into

* `"color"` attribute is in HEX format it has to be decimal format.
* `{{Name}}` must be in `<<<{{Name}}>>>`.
* Tweets typically need to remove inner's, they should look like `{"content":"<<<{{Url}}>>> <<<{{SourceUrl}}>>>"}`.
* `"icon_url"` must end in .png, .jpg, etc.
* Both, gif and timestamps aren't possible.
* `"timestamp":"{{EntryPublished}}",` will not work anymore.
* A blank message requires `{"content":""}`.
* Make sure you select the correct if ... then argument e.g. If RSS then Webhook, If Twitter then...


## Useful websites to help you with regex & Discord Embeds
* [RSS Bridge](https://github.com/RSS-Bridge/rss-bridge)
* [Working with Twitetr filters](http://followthehashtag.com/help/hidden-twitter-search-operators-extra-power-followthehashtag/)
* [Advance Twitter filters](https://developer.twitter.com/en/docs/tweets/rules-and-filtering/overview/standard-operators)
* [Discord + Ifttt step-by-step PDF Guide by Ben](https://mega.nz/#!uc5gHYZC!1dqXUlgMwtioJpcxYnhhS0rfYo2u2T8L1afpIOYtFuc)
* [Reddit.rss to Discord filter code + parser](https://gist.github.com/Birdie0/5830535877a94ab772efeb897e58e0e8)
* [Google-Forms-to-Discord](https://github.com/Iku/Google-Forms-to-Discord/blob/master/google%20script.js)


## Online formatters/validators
* [Regular Expressions 101](https://regex101.com/)
* [Visualizer and validator for Discord embeds](https://leovoel.github.io/embed-visualizer/) (_ensure you enable "webhook mode"_)
* [Unicode Text Converter](http://qaz.wtf/u/convert.cgi)
* https://codebeautify.org/jsonviewer
* https://jsonbeautifier.org/
* https://jsoneditoronline.org/
* https://jsonformatter-online.com/
* https://jsonformatter.curiousconcept.com/
* https://jsonformatter.org/
* https://www.jsonformatter.io/


## How a typical webhook must look like

**On the Ifttt website do the following**

* Go to `Webhooks` -> `Make a web request`
* Fill the fields with the values:
* URL - `Webhook url` _// The one you get from Discord Webhook menu_
* As Method choose: `POST`
* Content Type - `application/json`
* As Body, only one example: `{"content": "{{LinkToTweet}}"}` //This is the part you can freely change (see examples below)


## What does `Action failure message: Rate limited by the remote server` mean?

This means that Discord rate limited request because IFTTT sends it too frequently. This mostly happens when IFTTT tries to send a lot of requests on same webhook in short amount of time. **Discord's webhook rate limit is 5 requests per 2 seconds**. Unable to make web request. Your server returned a 400. 400 means request is invalid. Can be caused by:
- wrong method verb (should be POST)
- wrong content-type (should be application/json)
- bad request body:
- empty, not json or invalid json also it should be filled correctly, [check this for more info](https://birdie0.github.io/discord-webhooks-guide/).
- resulting json is broken (usually caused by newlines in ingredients (json doesn't support them in values) and unicode characters (rare but happens sometimes)), can be fixed by escaping variables with `<<<{{ingredient}}>>>` (_the website says to use <<double>>, ignore that_)
- error from server saying that one of fields hit limit (sadly, but ifttt doesn't show error that came from server). Try replace ingredients with data from applet logs and send it through Postman, Insomnia, other REST Client or using @glue's g.jp <JSON>, g.test <JSON> or g.webhook <JSON> commands (last one requires adding webhook through g.set webhook <url>).
* Error 401 - webhook url isn't full. Usually happens on phone where is hard to copy url from web version of discord. I've made this simple web app https://get-discord-webhook-url.herokuapp.com/ that allows you to create webhook on phone.
* Error 404 - url of webhook you're using has been removed. Means somebody removed it. Solution: create new webhook and replace old url.
* Error 405 - Happens when you use other than POST methods.


## Debug possible errors

**Where the IFTTT logs are?**

1. Open My Applets tab `https://ifttt.com/my_applets`
2. Click the applet you have problems with.
3. Click the gear icon in the corner.
4. Click `View activity log`.



### Advance YouTube Upload finished announce feed

```json
{
    "content": "Hello @everyone peeps, **<<<{{AuthorName}}>>>** has uploaded a new video! <<<{{Url}}>>>",
    "embeds": [
      {
        "title": "<<<{{Title}}>>>",
        "description": "<<<{{Description}}>>>",
        "url": "<<<{{Url}}>>>",
        "color": 6570404,
        "fields": [
          {
            "name": "Timestamp",
            "value": "<<<{{CreatedAt}}>>>",
            "inline": true
          },
          {
            "name": "Added By",
            "value": "Magic",
            "inline": true
          }
        ],
        "author": {
          "name": "<<<{{AuthorName}}>>>",
          "url": "https://www.youtube.com/channel/",
          "icon_url": "https://yt3.ggpht.com/a/AATXAJyI_DBeZc7O5cJ-Ih5ovltedR1HBIvcVH9ERQ=s100-c-k-c0xffffffff-no-rj-mo"
        },
        "footer": {
          "text": "Notice: Please be patient if the bot is broken, or there is no Application Officer around attending to you.",
          "icon_url": "https://yt3.ggpht.com/a/AATXAJyI_DBeZc7O5cJ-Ih5ovltedR1HBIvcVH9ERQ=s100-c-k-c0xffffffff-no-rj-mo"
        }
      }
    ]
}
```


### Android App Updates

```json
{
  "username":"App Update",
  "content":"<<<{{Name}}>>>(<<<{{Version}}>>>)\n <<<{{AppStoreUrl}}>>> \n Changelog: <<<{{ReleaseNotes}}>>>"
}
```


### Reddit Game Findings (_works basically with every Giveaway/Gift Subreddit_)

```json
{
    "embeds": [
    { "title": "<<<{{Title}}>>>",
    "url": "<<<{{PostURL}}>>>",
    "description": "<<<{{Content}}>>>",
    "thumbnail":
          {
          "url": "<<<{{ImageURL}}>>>" },
          "footer": { "icon_url": "https://www.redditstatic.com/desktop2x/img/favicon/favicon-32x32.png",
          "text": "/u/<<<{{Author}}>>> | <<<{{PostedAt}}>>>"
        }
      }
    ]
}
```

### Thank user for the follow on Twitter

```json
{ "content":"Thx For Following: @{{FullName}}\nFollow Count: {{FollowerCount}}" }
```


### Twitch went live Feed

```json
{
    "content": "{{ChannelName}} went live on Twitch",
    "embeds": [
    {
      "title": "{{ChannelUrl}}",
      "url": "{{ChannelUrl}}",
      "color": 6570404,
      "footer": {
      "text": "{{CreatedAt}}"
    },
      "image": {
      "url": "{{StreamPreview}}"
    },
      "author": {
      "name": "{{ChannelName}} is now streaming"
    },
      "fields": [
    {
      "name": "Playing",
      "value": "{{Game}}",
      "inline": true
    },
    {
      "name": "Started at (streamer timezone)",
      "value": "{{CreatedAt}}",
      "inline": true
      }
     ]
    }
  ]
}
```

### Twitch with viewer count & embed preview

```json
{
 "username":"{{ChannelName}}",
 "avatar_url":"https://avatar.glue-bot.xyz/twitch/%7B%7BChannelName%7D%7D",
 "embeds":[
   {
     "author":
     { "name": "CKstest",
     "url": "http://twitch.tv/CKstest",
     "icon_url": "https://static-cdn.jtvnw.net/jtv_user_pictures/4aecbebc-b161-40ba-843a-86a497fe25be-profile_image-300x300.png"
     },
     "title":" {{ChannelName}}", "url":" {{ChannelUrl}}",
     "description":"CKstest is now live!",
     "color":6570405,
     "thumbnail":
     {
       "url": "https://static-cdn.jtvnw.net/jtv_user_pictures/4aecbebc-b161-40ba-843a-86a497fe25be-profile_image-300x300.png"
     },
       "fields":[
         {
           "name":"Game", "value":" {{Game}}", "inline":true
         },
         {
           "name":"Viewers", "value":" {{CurrentViewers}}", "inline":true
           }
           ],
           "image":
           {
           "url":" {{StreamPreview}}"
     }
    }
  ]
}
 ```


### Twitter Basic Feed

```json
{"content":" <<<{{EntryTitle}}>>> <<<{{EntryContent}}>>> "}
```


### Twitter Advance Feed

```json
{
  "embeds": [
    {
      "description": "**CKsTechNews tweeted this at {{CreatedAt}}**\n\n> **content**\n\n\n\n",
      "color": 11927808,
      "fields": [
        {
          "name": "Link to Tweet",
          "value": "Click [here]({{LinkToTweet}}) to go to the original tweet."
        }
      ],
      "author": {
      "name": "{{UserName}} tweeted:",
      "url": "https://twitter.com/CKsTechNews/",
      "icon_url": "https://pbs.twimg.com/profile_images/1054491558643449857/PmCE4aeO_400x400.jpg"
      }
    }
  ]
}
```


### Twitter Advance Feed with Embed

```json
{
  "embeds": [
    {
      "title": "LINK",
      "color": 1942002,
      "url": "{{LinkToTweet}}",
      "author":{
        "name": "CKsTechNews (@CKsTechNews)",
        "url": "https://twitter.com/CKsTechNews/",
        "icon_url": "https://pbs.twimg.com/profile_images/1054491558643449857/PmCE4aeO_400x400.jpg"
        },
        "footer":{
          "text": "CKsTechNews! at {{CreatedAt}}!",
          "icon_url": "https://i.imgur.com/b4Nmq13.png"
        },
        "description": "CKsTechNews tweeted this : {{Text}}"
        }
      ]
}
```


### Instagram (complex)

```json
{
  "content":" :exclamation: @CKsTechNews just posted :exclamation: ",
  "embeds": [
    {
    "title": "<<<{{Title}}>>>",
    "description": "<<<{{Description}}>>>",
    "color":12662148,
    "url": "<<<{{Url}}>>>",
    "image":
    {
      "url": "<<<{{SourceUrl}}>>>"
    },
      "author":
    {
      "name":"CKsTechNews - Test",
      "url":"https://www.instagram.com/CKsTechNews/",
      "icon_url":"https://random-image-random_n.png",
      "footer":
    {
      "icon_url": "https://logodownload.org/wp-content/uploads/2017/04/instagram-logo.png",
      "text": "@CKStechNews <<<{{CreatedAt}}>>>"
    }
   }
  ]
}
```


### Instagram (simple)

```json
{
  "embeds":[
    {
      "title":"New Post From CKsTechTestWebHook on Instagram!",
      "description":" <<<{{Caption}}>>>",
      "color":12592603
    }
  ]
}
```


### Nitter (Twitter) Tweet vi role-id

```json
{
  "username": "<<<{{UserName}}>>>",
  "avatar_url": "https://pbs.twimg.com/profile_images/1054491558643449857/PmCE4aeO_400x400.jpg",
  "content": "<@&role-id>",
  "embeds": [
    {
    "title": "New Tweet!",
    "url": "<<<{{LinkToTweet}}>>>",
    "color": 33972,
    "description": "<<<{{Text}}>>>"
    }
  ]
}
```


### RSS Feed (Basic)

```json
{
  "embeds": [
    { "title": "{{Title}}", "url": "{{PostURL}}", "description": "{{Content}}" }
  ]
}
```


### RSS (Advance)

```json
{
  "content":"**Test**",
  "embeds": [
    {
      "title":"{{EntryTitle}}",
      "description":"{{EntryContent}}",
      "url":"https://discordapp.com",
      "color":14510884,
      "footer":{"icon_url":"https://cdn.discordapp.com/embed/avatars/0.png",
      "text":"{{FeedTitle}}"},
      "thumbnail":
      {
        "url":"https://cdn.discordapp.com/embed/avatars/0.png"},
        "author":
      {
        "name":"{{EntryAuthor}}",
        "url":"{{FeedUrl}}",
        "icon_url":"https://cdn.discordapp.com/embed/avatars/0.png"
      }
     }
   ]
}
```

## Old and unused stuff

### Pizza Delivery (_I do not use it anymore since YAGPDB has a reminder function_)

```json
{
  "embeds": [{
    "author": {
      "name": "CK's Delivery Girl",
      "url": "https://www.reddit.com/r/Pizza/",
      "icon_url": "https://i.imgur.com/V8ZjaMa.jpg"
    },
    "description": "Your pizza is ready!\n:timer:ETA: 10 minutes."
   }
  ]
}
```


### Tumblr

```json
{
   "username":"Bot Name",
   "avatar_url":"The URL of a Profile Picture/Thumbnail For The Bot Goes Here",
   "embeds":[
      {
         "title":"{{Title}}",
         "url":"{{Url}}",
         "description":"{{Body}}",
         "footer":{
            "text":"Uploaded On: {{Time}}"
         }
      }
   ]
}
```

### Facebook

I'm not active on Facebook (or not really), so I dropped it.

```json
{
  "username":"Bot Name",
  "avatar_url":"The URL of a Profile Picture/Thumbnail For The Bot Goes Here",
  "embeds": [
    {
      "title":"{{UpdatedAt}}",
      "url":"{{PageUrl}}",
      "author":
      {
        "name":"{{PageName}}",
        "url":"https://facebook.com/{{PageName}}"
      },
      "description":"{{Message}}"
      }
    ]
}
```


### SoundClood

Sadly I'm not really active on SC anymore (_no time_).

```json
{
    "content": "New track posted to SoundCloud",
    "embeds": [
        {
            "title": "<<<{{Title}}>>>",
            "url": "<<<{{TrackUrl}}>>>",
            "color": 16742144,
            "footer": {
                "text": "<<<{{Tags}}>>> || <<<{{CreatedAt}}>>>"
            },
            "thumbnail": {
                "url": "<<<{{ImageUrl}}>>>"
            },
            "author": {
                "name": "<<<{{Username}}>>>",
                "url": "<<<{{UserProfileUrl}}>>>",
                "icon_url": "<<<{{ImageUrl}}>>>"
            },
            "fields": [
                {
                    "name": "Description",
                    "value": "<<<{{Description}}>>>"
                }
            ]
        }
    ]
}
```


### GitHub Webhook

I replaced it with Yappy Bot. `timestamp` line might invalidate json.

```json
{
  "embeds": [
    {
      "title": "[<<<{{RepositoryName}}>>>] <<<{{PullRequestTitle}}>>>",
      "description": "<<<{{PullRequestBody}}>>>",
      "url": "<<<{{PullRequestURL}}>>>",
      "color": 21953,
      "timestamp": "<<<{{CreatedAt}}>>>",
      "author": {
        "name": "<<<{{AuthorUsername}}>>>",
        "url": "https://github.com/<<<{{AuthorUsername}}>>>",
        "icon_url": "<<<{{AuthorAvatarImageURL}}>>>"
      }
    }
  ]
}
```


### Yet another basic Twitter feed

```json
{
  "username": "@CKsTechNews tweeted",
  "content": "@USerName tweeted this {{CreatedAt}} : {{LinkToTweet}} "
}
```


### NASA - Image of the Day

```json
{
  "content": "NASA posted something new!",
  "embeds": [
    {
      "title": "<<<{{ImageTitle}}>>>",
    "description": "<<<{{Description}}>>>",
    "color":15193,
    "image":
    {
      "url": "<<<{{ImageSourceURL}}>>>"},
      "author":
      {
      "name":"Image of the Day by NASA"},
      "footer":
      {
        "text": "<<<{{PublishedDate}}>>>"
    }
    }
  ]
}
```


### Basic RSS

```json
{
  "embeds": [
    {
      "title": "? New Post ? ",
      "description": "<<<{{Caption}}>>>",
      "image": {
        "url": "<<<{{SourceUrl}}>>>"
      },
      "footer": {
        "text": "<<<{{CreatedAt}}>>>"
      },
      "url": "<<<{{Url}}>>>"
    }
  ]
}
```


### My first webhook ever (Twitter)

```json
{
    "content":"@<<<{{UserName}}>>> posted: <<<{{LinkToTweet}}>>> <<<{{TweetEmbedCode}}>>>"
}
```


## Unfinished


### Wind & Weather

Why is it unfinished? Because I found no way to create a secure webhook which does not expose the wunderground.com API key to Discord.

```json
{
  "content": "New Wind & Weather Update",
  "embeds": [
    {

      "color": 103900,



"fields": [
        {
        "name": "Current Temperature :thermometer:",
        "value": "{{CurrentTempCelsius}} C.",
        "inline": true

        },
        {
        "name": "Humidity::droplet: ",
        "value": "{{Humidity}}",
        "inline": true

        },
        {
        "name": "Max. Temperature Today::arrow_up: ",
        "value": "{{HighTempCelsius}}",
        "inline": false

        },

        {
        "name": "Min. Temperature Today::arrow_down: ",
        "value": "{{LowTempCelsius}}",
        "inline": true
        },

        {
        "name": "CurrentCondition::sunglasses:",
        "value": "{{CurrentCondition}}",
        "inline": true

        },
        {
        "name": "WindSpeed::wind_chime:",
        "value": "{{WindSpeedKph}} Kph",
        "inline": true

        }
      ]
    },
    {
      "color": 12845619,
      "title": "How to cancel your subscription:",
      "description": "Cancel your subscription to daily weather updates by unreacting to the bell icon in #announcements. \n**NOTE**: Updates are directly posted from: **https://www.wunderground.com/**."
    }
  ]
}
```
