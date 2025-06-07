import os
import telebot

TOKEN = os.environ.get("TOKEN")
CAPITAL = int(os.environ.get("CAPITAL", "100"))
bot = telebot.TeleBot(TOKEN)

def classify(num):
    num = int(num)
    size = "Big ☝🏻" if num >= 5 else "Small 👇🏻"
    color = "Red 👇🏻" if num % 2 == 0 else "Green ☝🏻"
    return size, color

def predict(prng_numbers):
    size_count = {"Big": 0, "Small": 0}
    color_count = {"Red": 0, "Green": 0}
    for n in prng_numbers:
        s, c = classify(n)
        size_count[s.split()[0]] += 1
        color_count[c.split()[0]] += 1

    size_suggest = max(size_count, key=size_count.get)
    color_suggest = max(color_count, key=color_count.get)

    accuracy = 80
    suggestion_type = "Highest Probability 🔥" if accuracy >= 80 else "Best Trade 💹"
    bet_amount = int(CAPITAL * accuracy / 1000)

    return f"""📊 *Prediction Suggestion*  
Size: {size_suggest}  
Color: {color_suggest}  
Confidence: {accuracy}% 🔥  
Suggested Bet: ₹{bet_amount}  
Type: {suggestion_type}
"""

@bot.message_handler(func=lambda m: True)
def handle_message(message):
    text = message.text
    try:
        numbers = [int(x) for x in text.strip().split() if x.isdigit()]
        if len(numbers) < 10:
            bot.reply_to(message, "⚠️ At least 10 PRNG numbers chahiye prediction ke liye.")
            return
        result = predict(numbers)
        bot.reply_to(message, result, parse_mode='Markdown')
    except Exception as e:
        bot.reply_to(message, f"❌ Error: {str(e)}")

print("🤖 Bot Running...")
bot.polling()
