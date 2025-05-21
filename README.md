import requests
import time

# Telegram bot məlumatları
TOKEN = 'your_token'
CHAT_ID = 'your_idchat'

# Binance API URL
BINANCE_URL = 'https://api.binance.com/api/v3/ticker/price?symbol=ADAUSDT'

# Binance-dən ADA qiymətini alır
def get_ada_price():
    try:
        response = requests.get(BINANCE_URL)
        data = response.json()
        return data['price']
    except Exception as e:
        return f"Xəta baş verdi: {e}"

# Telegrama mesaj göndərir
def send_telegram_message(message):
    url = f'https://api.telegram.org/bot{TOKEN}/sendMessage'
    payload = {
        'chat_id': CHAT_ID,
        'text': message
    }
    try:
        requests.post(url, data=payload)
    except Exception as e:
        print(f"Mesaj göndərilərkən xəta baş verdi: {e}")

# Əsas döngü (10 dəqiqədən bir yenilənir)
def main():
    while True:
        price = get_ada_price()
        message = f"💰 ADA qiyməti: {price} USDT"
        send_telegram_message(message)
        time.sleep(60)  # 10 dəqiqə = 60 saniyə

if __name__ == '__main__':
    main()
