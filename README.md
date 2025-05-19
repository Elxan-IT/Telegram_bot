# Telegram_bot
import requests
from bs4 import BeautifulSoup
from openpyxl import Workbook
import time
from telegram import Bot

#Telegram-Bot
BOT_TOKEN = "7961154905:AAE39dcHN3GqA-la8WSJlboHTJLSHTtaH_4"
CHAT_ID = "5882097855"

bot = Bot(token=BOT_TOKEN)

#Define URL (Book)

def scraping_and_sent():
   url="https://emlak.az/"
   time.sleep(5)

   headers = {"User-Agent": "Mozilla/5.0"}
   response = requests.get(url, headers=headers)
   soup = BeautifulSoup(response.text, "html.parser")

# Scrape listing
   emlak=soup.find_all("div", class_="ticket-item")[1:2]


   for product in emlak:

    price_tag = product.find("div", class_="price-ticket")
    attribute_tag = product.find("div", class_="description-ticket")


    price=price_tag.get_text(strip=True) if price_tag else ""
    attribute=attribute_tag.get_text(strip=True) if attribute_tag else ""

#
    mesaj = f"""
🏠  <b>{attribute}</b>
💰 Qiymət: <i>{price}</i>
"""
    bot.send_message(chat_id=CHAT_ID, text=mesaj, parse_mode="HTML")

while True:
  scraping_and_sent()
  time.sleep(86400)
