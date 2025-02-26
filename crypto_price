import telebot
import requests
import time

# 🔹 توکن ربات تلگرام (از BotFather دریافت کنید)
TOKEN = "7204338966:AAFLYgQePQhhk7aRtRkLYo2cvo9CtrL2q4U"

# 🔹 نام کاربری کانال (بدون @)
CHAT_ID = "@nabze_market"

# 🔹 تعریف ربات تلگرام
bot = telebot.TeleBot(TOKEN)

# 🔹 لیست ارزهای دیجیتال که می‌خواهیم قیمت‌شان را بگیریم
CRYPTO_LIST = ["bitcoin", "ethereum"]

# 🔹 تابع دریافت قیمت‌ها از CoinGecko
def get_crypto_prices():
    url = f"https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids={','.join(CRYPTO_LIST)}"
    response = requests.get(url)
    data = response.json()
    return data

# 🔹 حلقه برای ارسال قیمت‌ها هر 60 ثانیه
while True:
    # دریافت قیمت‌ها از CoinGecko
    prices = get_crypto_prices()

    # 🔹 ساخت پیام برای ارسال به کانال
    message = "💰 :آخرین قیمت لحظه ای ارزهای دیجیتال:\n"
    for crypto in CRYPTO_LIST:
        name = crypto.capitalize()  # تبدیل اولین حرف به بزرگ
        price = prices[CRYPTO_LIST.index(crypto)]["current_price"]
        
        # دریافت تغییرات 24 ساعته
        price_change_24h = prices[CRYPTO_LIST.index(crypto)].get("price_change_percentage_24h", 0)

        # اضافه کردن قیمت و درصد تغییرات
        if price_change_24h < 0:
            message += f"🔥 {name}: ${price} USD 🔥\n"
            message += f"📈 تغییرات 24 ساعته: منفی {abs(price_change_24h)}%\n"
        else:
            message += f"🔥 {name}: ${price} USD 🔥\n"
            message += f"📈 تغییرات 24 ساعته: مثبت {price_change_24h}%\n"

        # اضافه کردن خط چین بعد از هر ارز
        message += "--------------------\n"

    # ✅ اضافه کردن هشتگ‌های مرتبط در انتهای پیام
    hashtags = "\n📌 #بیتکوین #اتریوم #کریپتو #ارزدیجیتال #سرمایه_گذاری"
    message += hashtags

    # ✅ ارسال پیام به کانال تلگرام با استفاده از HTML
    try:
        bot.send_message(CHAT_ID, message, parse_mode='HTML')
    except Exception as e:
        print(f"Error occurred: {e}")

    # 🔹 صبر کردن 60 ثانیه قبل از ارسال مجدد
    time.sleep(60)
