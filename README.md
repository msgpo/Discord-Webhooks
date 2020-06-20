# Discord <-> Ifttt Webhooks

Just my little collection of [Ifttt.com](https://ifttt.com/my_applets) apps which are linked with my [Discord webhooks](https://support.discord.com/hc/en-us/articles/228383668-Webhooks-gebruiken).


## Common mistakes I (_and others_) run into

* `"color"` attribute is in HEX format it has to be decimal format.
* `{{Name}}` must be in `<<<{{Name}}>>>`.
* Tweets typically need to remove inner's, they should look like `{"content":"<<<{{Url}}>>> <<<{{SourceUrl}}>>>"}`.
* `"icon_url"` must end in .png, .jpg, etc.


## Useful websites to help you with regex & Discord Embeds
* [https://regex101.com/](https://regex101.com/)
* [https://leovoel.github.io/embed-visualizer](https://leovoel.github.io/embed-visualizer/)
* [RSS Bridge](https://github.com/RSS-Bridge/rss-bridge)
* [Working with Twitetr filters](http://followthehashtag.com/help/hidden-twitter-search-operators-extra-power-followthehashtag/)
* [Advance Twitter filters](https://developer.twitter.com/en/docs/tweets/rules-and-filtering/overview/standard-operators)
* [Discord + Ifttt step-by-step PDF Guide by Ben](https://mega.nz/#!uc5gHYZC!1dqXUlgMwtioJpcxYnhhS0rfYo2u2T8L1afpIOYtFuc)


## How a typical webhook must look like

**On the website do the following**

* `Webhooks` -> `Make a web request`
* Fill the fields with the values:
* Url - `Webhook url` // The one you get from Discord Webhook menu
* As Method choose: `POST`
* Content Type - `application/json`
* As Body, only one example: `{"content": "{{LinkToTweet}}"}` //This is the part you can freely change (see examples below)


## What does `Action failure message: Rate limited by the remote server` mean?

This means that Discord rate limited request because IFTTT sends it too frequently. This mostly happens when IFTTT tries to send alot of requests on same webhook in short amount of time. **Discord's webhook ratelimit is 5 requests per 2 seconds**. Unable to make web request. Your server returned a 400. 400 means request is invalid. Can be caused by:
- wrong method verb (should be POST)
- wrong content-type (should be application/json)
- bad request body:
- empty, not json or invalid json also it should be filled correctly, [check this for more info](https://birdie0.github.io/discord-webhooks-guide/).
- resulting json is broken (usually caused by newlines in ingredients (json doesn't support them in values) and unicode characters (rare but happens sometimes)), can be fixed by escaping variables with <<<{{ingredient}}>>> (website says to use <<double>>, ignore that)
- error from server saying that one of fields hit limit (sadly, but ifttt doesn't show error that came from server). Try replace ingredients with data from applet logs and send it through Postman, Insomnia, other REST Client or using @glue's g.jp <JSON>, g.test <JSON> or g.webhook <JSON> commands (last one requires adding webhook through g.set webhook <url>).
* Error 401 - webhook url isn't full. Usually happens on phone where is hard to copy url from web version of discord. I've made this simple web app https://get-discord-webhook-url.herokuapp.com/ that allows you to create webhook on phone.
* Error 404 - url of webhook you're using has been removed. Means somebody removed it. Solution: create new webhook and replace old url.
* Error 405 - Happens when you use other than POST methods.

## Debug errors

**Where the IFTTT logs are?**

1. Open My Applets tab `https://ifttt.com/my_applets`
2. Click the applet you have problems with.
3. Click the gear icon (⚙️) in the corner.
4. Click `View activity log`.

### Advance YouTube Upload finished announce feed

```bash
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

### Reddit Game Findings (_works basically with every Giveaway/Gift Subreddit_)

```bash
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

```bash
{ "content":"Thx For Following: @{{FullName}}\nFollow Count: {{FollowerCount}}" }
```


### Twitch went live Feed

```bash
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

```bash
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
           { "name":"Viewers", "value":" {{CurrentViewers}}", "inline":true
           }
           ],
           "image":
           { "url":" {{StreamPreview}}"
           }
           }
           ]
           }
 ```


### Twitter Advance Feed

```bash
{
  "embeds": [
    {
      "description": "**CKsTechNews tweeted this at {{CreatedAt}}**\n\n> **content**\n\n\n\n",
      "color": 11927808,
      "fields": [
        {
          "name": "Link to Tweet",
          "value": "Click [here]({{LinkToTweet}}) to go to  the original tweet"
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


### Instagram

```bash
{
  content":":exclamation: @CKsTechNews just posted:exclamation: ",
    "embeds": [
    {
    "title": "<<<{{Title}}>>>",
    "description": "<<<{{Description}}>>>",
    "color":"12662148",
    "url": "<<<{{Url}}>>>",
    "image":
    {
      "url":" {{SourceUrl}}"
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

### Nitter Tweet vi role-id

```bash
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
