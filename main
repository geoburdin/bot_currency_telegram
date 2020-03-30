import requests # Модуль для обработки URL
from bs4 import BeautifulSoup # Модуль для работы с HTML
import time # Модуль для остановки программы
import telebot
TOKEN = ""
bot = telebot.TeleBot(TOKEN)

class Currency:
    DOLLAR_RUB="https://www.google.ru/search?client=opera&q=курс+доллара+к+рублю&sourceid=opera&ie=UTF-8&oe=UTF-8"
    headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36 OPR/67.0.3575.97'}
    current_converted_price=0
    difference=1

    def __init__(self):
        self.current_converted_price=float(self.get_currency_price().replace(",", "."))

    def get_currency_price(self):
        # Парсим всю страницу
        full_page = requests.get(self.DOLLAR_RUB, headers=self.headers)

        # Разбираем через BeautifulSoup
        soup = BeautifulSoup(full_page.content, 'html.parser')

        # Получаем нужное для нас значение и возвращаем его
        convert = soup.findAll("span", {"class": "DFlfde SwHCTb"})
        return convert[0].text

    def __str__(self):
        return str(self.current_converted_price)

@bot.message_handler(commands=["start"])
def send_welcome(message):
    id_chat = message.chat.id
    bot.send_message(id_chat,"Приветствую! Я - валютный бот" )

@bot.message_handler( commands= ["dollar"])
def send_dollar(message):
    id_chat = message.chat.id
    currency = str(Currency())
    bot.send_message(id_chat, "Сейчас курс: 1 доллар = " + currency)

@bot.message_handler( commands= ["dollar_repeat"])
def send_dollar_repeat(message):
    id_chat = message.chat.id

    currency = str(Currency())
    id_m=bot.send_message(id_chat, "Сейчас курс: 1 доллар = " + currency).message_id
    while True:
        try:
            if currency != str(Currency()):
                currency = str(Currency())
                bot.edit_message_text("Сейчас курс: 1 доллар = "+ currency, id_chat, id_m)
                bot.send_message(id_chat, "исправлено " + currency)
                time.sleep(10)
                break
        except Exception:
            bot.send_message(id_chat, "ошибка " + currency)
@bot.message_handler( commands= ["stop"])
def stop_repeat(message):
    id_chat = message.chat.id
    bot.stop_bot()
bot.polling()

