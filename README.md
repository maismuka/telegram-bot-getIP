# telegram-bot-getIP

# Requirements

1. VPS Server
2. A Telegram bot


## Set Up Your Bot

Search for the “botfather” telegram bot (he’s the one that’ll assist you with creating and managing your bot)

Type /help to see all possible commands the botfather can handle

Click on or type /newbot to create a new bot then follow instructions and make a new name for your bot

Congratulations! You have created your first bot. You should see a new API token.
For example `777845702:AAFdPS_taJ3pTecEFv2jXkmbQfeOqVZGER`

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
##
import fcntl
import struct
##

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
    ss = socket.inet_ntoa(fcntl.ioctl(
        s.fileno(),
        0x8915,  # SIOCGIFADDR
        struct.pack('256s', 'eth0'[:15])
    )[20:24])
    bot.send_message(chat_id=update.message.chat_id, text=str(ss))
    







