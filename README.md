# -RhythmicRealm01Bot
from telegram.ext import Updater, CommandHandler
from ytmusicapi import YTMusic

def start(update, context):
    update.message.reply_text("Ви можете використовувати команду /search для пошуку музики.")

def search_music(update, context):
    query = ' '.join(context.args)
    if query:
        ytmusic = YTMusic()
        search_results = ytmusic.search(query)

        if search_results and 'videos' in search_results:
            first_result = search_results['videos'][0]
            title = first_result['title']
            url = f"https://www.youtube.com/watch?v={first_result['videoId']}"
            update.message.reply_text(f"Знайдено: {title}\n{url}")
        else:
            update.message.reply_text("Музика не знайдена.")
    else:
        update.message.reply_text("Будь ласка, введіть запит для пошуку музики.")

def main():
    updater = Updater("6453624180:AAHq6QWkhfWUqxiHMsPdk8NIj1-Kp5rXOHk", use_context=True)
    dp = updater.dispatcher

    dp.add_handler(CommandHandler("start", start))
    dp.add_handler(CommandHandler("search", search_music, pass_args=True))

    updater.start_polling()
    updater.idle()

if name == '__main__':
    main()
