# Telegram_getUpdates-Long_polling-


import configparser
import json

from telethon import TelegramClient
from telethon.errors import SessionPasswordNeededError


# Reading Configs
config = configparser.ConfigParser()
config.read("config.ini")

# Setting configuration values(file config.ini)
api_id = config['Telegram']['api_id']
api_hash = config['Telegram']['api_hash']

api_hash = str(api_hash)

phone = config['Telegram']['phone']
username = config['Telegram']['username']

import tracemalloc
# Create the client and connect
client = TelegramClient(username, api_id, api_hash)
client.start()
print("Client Created")
# Ensure you're authorized
if not client.is_user_authorized():
    client.send_code_request(phone)
    try:
        client.sign_in(phone, input('Enter the code: '))
    except SessionPasswordNeededError:
        client.sign_in(password=input('Password: '))
        
        
import tracemalloc
tracemalloc.start()
offset_id = 0
limit = 100
all_messages = []
total_messages = 0
total_count_limit = 0

while True:
    print("Current Offset ID is:", offset_id, "; Total Messages:", total_messages)
    history = client(GetHistoryRequest(
        peer='name_channel',
        offset_id=42,
        offset_date=datetime.datetime(2020, 8, 9),
        add_offset=0,
        limit=100,
        max_id=0,
        min_id=0,
        hash=0
    ))
    if not history.messages:
        break
    messages = history.messages
    for message in messages:
        all_messages.append(message.to_dict())
    offset_id = messages[len(messages) - 1].id
    total_messages = len(all_messages)
    if total_count_limit != 0 and total_messages >= total_count_limit:
        break
