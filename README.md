# telegram-scout-botimport os
import time
from telegram import Bot, Update
from telegram.ext import Updater, MessageHandler, Filters

BOT_TOKEN = os.environ.get("BOT_TOKEN")
USER_CHAT_ID = 6492537911  # your personal Telegram ID

bot = Bot(BOT_TOKEN)

# CherryTrending only (safe start)
MONITORED_CHATS = {
    -100XXXXXXXXXX: ["buy", "trending", "pump", "new", "hot"]
}

def handle_channel(update: Update, context):
    message = update.channel_post
    if not message or not message.text:
        return

    chat_id = message.chat_id
    text = message.text.lower()

    if chat_id in MONITORED_CHATS:
        if any(k in text for k in MONITORED_CHATS[chat_id]):
            bot.send_message(
                chat_id=USER_CHAT_ID,
                text=f"🍒 CherryTrending Alert:\n\n{text}"
            )

updater = Updater(BOT_TOKEN, use_context=True)
dp = updater.dispatcher

for chat_id in MONITORED_CHATS:
    dp.add_handler(
        MessageHandler(Filters.chat(chat_id=chat_id), handle_channel)
    )

print("Bot started on Railway")
updater.start_polling()
updater.idle()
