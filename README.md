import requests
from bs4 import BeautifulSoup
import time
from telegram import Bot

# Telegram Bot məlumatları
BOT_TOKEN = "7961154905:AAE39dcHN3GqA-la8WSJlboHTJLSHTtaH_4"
CHAT_ID = "5882097855"

bot = Bot(token=BOT_TOKEN)

def scraping_and_sent():
    url = "https://www.topaz.az/"
    headers = {"User-Agent": "Mozilla/5.0"}

    response = requests.get(url, headers=headers)
    soup = BeautifulSoup(response.text, "html.parser")

    # Əsas xəbərlərdən birini götürürük (ilk xəbər)
    news_item = soup.find("div", class_="main-news-item")
    if news_item:
        title = news_item.find("h3").get_text(strip=True) if news_item.find("h3") else "Başlıq tapılmadı"
        description = news_item.find("p").get_text(strip=True) if news_item.find("p") else "Açıqlama yoxdur"
        link = news_item.find("a")["href"] if news_item.find("a") else "#"

        mesaj = f"""
<b>Topaz Xəbəri</b>
📰 <b>{title}</b>
🗒️ {description}
🔗 <a href="https://www.topaz.az{link}">Ətraflı oxu</a>
        """
        bot.send_message(chat_id=CHAT_ID, text=mesaj, parse_mode="HTML")
    else:
        bot.send_message(chat_id=CHAT_ID, text="Xəbər tapılmadı.", parse_mode="HTML")

# Gündə 1 dəfə işlətmək üçün döngü
while True:
    scraping_and_sent()
    time.sleep(86400)  # 24 saat