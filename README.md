		_   _  _         _     ______  _              
	   | | | |(_)       | |    |  ___|(_)             
	   | |_| | _   __ _ | |__  | |_    _ __   __  ___ 
       |  _  || | / _` || '_ \ |  _|  | |\ \ / / / _ \
       | | | || || (_| || | | || |    | | \ V / |  __/
       \_| |_/|_| \__, ||_| |_|\_|    |_|  \_/   \___|
                   __/ |                              
                  |___/                               
        The Slack bot that brings your team together!

HighFive is a recognition system for Slack. Your team members reward each others and win gifts.

**Born and bread in Texas**

## How it all works

​Every day, each team member receives five :highfive: that can be used to reward a fellow colleague. Send recognition in a channel (public or private).

> ​A big thanks :highfive: to @mike for completing the merger!

Mike will receive a direct message from the bot informing him of the recognition.

> @larry gave you a :highfive:

And the sender will receive a confirmation

> You gave a :highfive: to @mike

When you have enough :highfive:s, you can exchange them for gifts.

You can configure all the gifts you want in HighFive, some ideas:

- Gift Cards (Amazon, Starbucks, iTunes, etc.)
- Lunch with CEO
- Anything you can imagine!

### Rules

- You can give several rewards (​:hatching_chick:​ ​:hatching_chick:​to @john)
- ​You can give reward to several people (​:hatching_chick:​ to @john and @jim)
- You can not give reward you you dont have !
- ​You can accumulate :hatching_chick: (but a maximum is set)
- You can not give reward you received (:hatched_chick:)
- ​You can not give :hatching_chick: in a direct message.

##Install
###Clone git repo
```
git clone https://github.com/helloroughton/highfive
```
###Install modules

Install all the libraries needed by node and HighFivebot.

```
npm install
```

###Configure your bot and get your token

[In Slack, create a bot](https://slack.com/apps/build/custom-integration) named `highfive` and copy the API token.

**Invite fighfive in each channel** needed to be monitored by your bot. You also need to create a private chanel `highfiveadmin` and invite your bot inside with `/invite highfive`.

If you want to make reward easier, [create a alias](https://slack.com/customize/emoji) `:thanks:` for the one you use `:hatching_chick:` by default (:hatching_chick:)

## Personalize you Bot

### Config file

Duplicate the `config/highfivebot.json` file, then launch your bot with the environnement `HighFiveBotConfig=new_conf_file.json` .

### Common section

- **botName** : name of the bot, as in Slack configuration
- **botTZ** : Time Zone of the Bot (currently unused)
- **cronHourUTC** and **cronMinuteUTC** : time of the reward delivery
- **giftHeardEmoji**  : emoji added in reaction, when a reward is noticed by the bot ( :robot_face: )
- **adminChanel** : private channel use to notify admin when a use get his gift (*highfiveadmin* by default)
- **adminUser** : list of admin users (currently unused)
- **excludedUser** : list of users that will not take part of the system
- **excludedChannel** : list of channels where the bot can not be invited
- **restrictedToUser** : only this users will take part (very usefull for test period)
- **restrictedToChannel** : if you want to limit the channels
- **utterances** : word used to say yes/no in bot conversation


### Reward section

- **recognitionByDay** : how many rewards are received each day (default : 1)
- **recognitionMax** : how many rewards can be cumulated (default 3)
- **excludedDays** : rewards are not given on those days (0 is sunday) (default weekend [6,0])
- **giftEmoji** : emoji used to give recognition (default 🐣)
- **giftEmojiAlias** : alias to make easy recognition (default `:thanks:` )
- **rewardEmoji** : emoji used to represent rewards (default :hatched_chick:)

### RewardRL section

Lists the gift that can be obtain in exchange of rewards :hatched_chick:

- **cost** : nomber of rewards required
- **desc** : gift description
- **getinfo** : explain how to get the gift in read life !
- **color** : hex color for the gifts list
- **thumb_url** : image url (size : 75px x 75px) for the gift list


##Launch
###Test
HighFiveBotToken=xoxp-123456-789012-??????-?????? node highfivebot.js
###Install as a daemon
Ìnstall [forever](https://www.npmjs.com/package/forever) to make HighFiveBot persistent.
```
[sudo] npm install forever -g
```
Create `/etc/init.d/highfivebot`.
Change the **HighFiveBotToken** and the **sourceDir**

```
#!/bin/sh
### BEGIN INIT INFO
# Provides:             highfivebot
# Required-Start:       $syslog $remote_fs
# Required-Stop:        $syslog $remote_fs
# Should-Start:         $local_fs
# Should-Stop:          $local_fs
# Default-Start:        2 3 4 5
# Default-Stop:         0 1 6
# Short-Description:    HighFive Bot
# Description:          The Slack bot that brings your team together!
### END INIT INFO

#/etc/init.d/highfivebot

export PATH=$PATH:/usr/local/bin
export NODE_PATH=$NODE_PATH:/usr/local/lib/node_modules
# change the token below with yours
export HighFiveBotToken=xoxb-12345678-azertyuiopQSDFGHJKLM 

case "$1" in
  start)
  # change --sourceDir to set the path where you install highfivebot
  exec forever --uid="dairybot" --sourceDir=/home/fermier/highfivebot -p /var/run/fermier.pid highfivebot.js 2>&1 > /dev/null &
  ;;
stop)
  exec forever stop highfivebot 2>&1 > /dev/null
  ;;
*)
  echo "Usage: /etc/init.d/highfivebot {start|stop}"
  exit 1
  ;;
esac

exit 0
```

Launch the daemon
```
/etc/init.d/highfivebot start
```


###Restart

You can restart the daemon in command line to reload the configuration
```
/etc/init.d/highfivebot stop
/etc/init.d/highfivebot start
```

Or DM your bot in Slack, and say `restart` and confirm `yes`

## FAQ

### HighFive Bot didn't see some rewards

The bot need to be invited in each channel public or private. You can also check if the bot is launched (the green bullet)

The bot can not see if you try to give reward through a direct message, Slack didn't allow it.

###Library

HighFiveBot is based on the Botkit library https://howdy.ai/botkit/
