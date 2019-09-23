# telegram-bot-getIP

This is a simple coding for Telegram Bot for returning local IP address.

My current problem is when my Raspberry Pi connecting to random WiFi and I unable to opbtain IP address.

After this tutorial, we might obtain user chat ID and bot IP local server

This simple Telegram Bot coding is the solution!


# Requirements

1. VPS Server
2. A Telegram bot


## Set Up Your Bot

Search for the `botfather` telegram bot (he’s the one that’ll assist you with creating and managing your bot)

Type `/help` to see all possible commands the botfather can handle

Click on or type `/newbot` to create a new bot then follow instructions and make a new name for your bot

Congratulations! You have created your first bot. You should see a new API token.
For example `777845702:AAFdPS_taJ3pTecEFv2jXkmbQfeOqVZGER`

Then, obtain your personal chat id for your Bot to communicate into

Search your bot and chat anything. There will be no response.

Copy and paste this url :
`https://api.telegram.org/bot<BOT TOKEN>/getUpdates`

Replace <BOT TOKEN> with your token earlier. For example:
```https://api.telegram.org/bot777845702:AAFdPS_taJ3pTecEFv2jXkmbQfeOqVZGER/getUpdates```

Look for `chat` object. It will be something like this `158328302`

## Set Up Bot Server

On your VPS server, run `sudo apt update && sudo apt -y upgrade`

Install PIP for Python Installer `apt install python3-pip`

Proceed to install framework from https://github.com/python-telegram-bot/python-telegram-bot

`pip3 install python-telegram-bot --upgrade`

Create a new python file for example `bot_telegram_getIP.py`

`nano bot_telegram_getIP.py`

Edit the file :

```
import logging
import socket
import telegram
import config
import fcntl
import struct

from telegram.ext import CommandHandler, Updater

logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
                     level=logging.INFO)

updater = Updater(token=config.TOKEN)
dispatcher = updater.dispatcher

# Commands:
def start(bot, update):
    if update.message.chat_id in config.CHATIDLIST:
        bot.send_message(chat_id=update.message.chat_id, text="I'm a bot, please talk to me!")

def getid(bot, update):
        bot.send_message(chat_id=update.message.chat_id, text=str(update.message.chat_id))   

def getip(bot, update):
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    s.connect(("8.8.8.8", 80))
    ss = s.getsockname()[0]
    bot.send_message(chat_id=update.message.chat_id, text=str(ss))
    
# Handlers
start_handler = CommandHandler('start', start)
dispatcher.add_handler(start_handler)

getid_handler = CommandHandler('getid', getid)
dispatcher.add_handler(getid_handler)

getip_handler = CommandHandler('getip', getip)
dispatcher.add_handler(getip_handler)

#if __name__ == 'main':
print('IP Bot Started')
updater.start_polling()
```

Now your Bot will have the basic function `/start`, `/getid` and `/getip`

`/start` will return chat message `I'm a bot, please talk to me!`

`/getid` will return the messanger chat ID

`/getip` will return the local ip for the Bot server


Now to create another file name `config.py`
`nano config.py`

Edit file:

```
CHATIDLIST=[<CHAT ID>] #int list
TOKEN='<BOT TOKEN>' #str
```

Replace both `<CHAT ID>` and `<BOT TOKEN>` into your id and token.


## Run Your Code

Run the code `bot_telegram_getIP.py` earlier:

`python3 bot_telegram_getIP.py`

Start communicating with your Bot

In your Telegram, send message to your bot using command created earlier.


## Run Python in Background Process
