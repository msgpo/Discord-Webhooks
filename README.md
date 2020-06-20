# Discord-Webhooks

Just my collection of [Ifttt.com](https://ifttt.com/my_applets) apps which are linked together with [Discord webhooks](https://support.discord.com/hc/en-us/articles/228383668-Webhooks-gebruiken) right into my server. 


## Common mistakes I (_and others_) often make

* "color" is in HEX format, has to be decimal format!
* `{{Name}}` must be in `<<<{{Name}}>>>`.
* Tweets typically need to remove inner's, they should look like `{"content":"<<<{{Url}}>>> <<<{{SourceUrl}}>>>"}`.


## Useful websites to help you with regex & Discord Embeds
* [https://regex101.com/](https://regex101.com/)
* [https://leovoel.github.io/embed-visualizer](https://leovoel.github.io/embed-visualizer/)
* [RSS Bridge](https://github.com/RSS-Bridge/rss-bridge)
* [Working with Twitetr filters](http://followthehashtag.com/help/hidden-twitter-search-operators-extra-power-followthehashtag/)
* [Advance Twitter filters](https://developer.twitter.com/en/docs/tweets/rules-and-filtering/overview/standard-operators)


## How a typical webhook must look like 

**On the website do the following**

* `Webhooks` -> `Make a web request`
* Fill the fields with the values:
* Url - `Webhook url` // The one you get from Discord Webhook menu
* As Method choose: `POST`
* Content Type - `application/json`
* As Body, only one example: `{"content": "{{LinkToTweet}}"}` //This is the part you can freely change (see examples below)


## What does `Action failure message: Rate limited by the remote server` mean?

This means that Discord ratelimited request because IFTTT sends it too frequently. This mostly happens when IFTTT tries to send alot of requests on same webhook in short amount of time. Discord's webhook ratelimit is 5 requests per 2 seconds. Unable to make web request. Your server returned a 400. 400 means request is invalid. Can be caused by:
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

### YouTube Upload finished

```bash
{
    "content": "Hello everyone peeps, **{{AuthorName}}** has uploaded a new video!",
    "embeds": [
      {
        "title": "{{Title}}",
        "description": "{{Description}}",
        "url": "{{Url}}",
        "color": 2407171,
        "fields": [
          {
            "name": "Timestamp",
            "value": "{{CreatedAt}}",
            "inline": true
          },
          {
            "name": "Added By",
            "value": "Magic",
            "inline": true
          }
        ],
        "author": {
          "name": "{{AuthorName}}",
          "url": "link",
          "icon_url": "link"
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
    "embeds": [{
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

