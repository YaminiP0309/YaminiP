from tkinter import *
import datetime
import requests

# GUI
root = Tk()
root.title("Chatbot")

BG_GRAY = "#ABB2B9"
BG_COLOR = "#17202A"
TEXT_COLOR = "#EAECEE"

FONT = "Helvetica 14"
FONT_BOLD = "Helvetica 13 bold"

# Send function
def send():
    user_input = e.get().lower()
    send_message("You -> " + user_input)

    response = generate_response(user_input)
    send_message("Bot -> " + response)

    e.delete(0, END)

def send_message(message):
    txt.insert(END, "\n" + message)

def generate_response(user_input):
    if user_input == "hello":
        return "Hi there, how can I help?"
    elif user_input in ["hi", "hii", "hiiii"]:
        return "Hi there, what can I do for you?"
    elif user_input == "how are you":
        return "Fine! And you?"
    elif user_input in ["fine", "i am good", "i am doing good"]:
        return "Great! How can I help you?"
    elif user_input in ["thanks", "thank you", "now its my time"]:
        return "My pleasure!"
    elif user_input in ["what do you sell", "what kinds of items are there", "have you something"]:
        return "We have coffee and tea"
    elif user_input in ["tell me a joke", "tell me something funny", "crack a funny line"]:
        return "What did the buffalo say when his son left for college? Bison.!"
    elif user_input in ["goodbye", "see you later", "see yaa"]:
        return "Have a nice day!"
    elif "time" in user_input:
        return get_current_time_response()
    elif "weather" in user_input:
        return get_weather_response()
    else:
        return "Sorry! I didn't understand that"

def get_current_time_response():
    current_time = datetime.datetime.now().strftime("%H:%M:%S")
    return f"The current time is {current_time}"

def get_weather_response():
    # Example: Fetch weather from a public API (OpenWeatherMap)
    api_key = "a03d198f432ac9511ea36799222756bf" 
    city = "Dindigul"  # Replace with your city
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric"
    try:
        response = requests.get(url)
        data = response.json()
        if data["cod"] == 200:
            temperature = data["main"]["temp"]
            description = data["weather"][0]["description"]
            return f"The weather in {city} is {description} with a temperature of {temperature}°C"
    except Exception as e:
        print("Error fetching weather:", e)
    return "Sorry, I couldn't fetch the weather information at the moment."

lable1 = Label(root, bg=BG_COLOR, fg=TEXT_COLOR, text="Welcome", font=FONT_BOLD, pady=10, width=20, height=1).grid(
    row=0)

txt = Text(root, bg=BG_COLOR, fg=TEXT_COLOR, font=FONT, width=60)
txt.grid(row=1, column=0, columnspan=2)

scrollbar = Scrollbar(txt)
scrollbar.place(relheight=1, relx=0.974)

e = Entry(root, bg="#2C3E50", fg=TEXT_COLOR, font=FONT, width=55)
e.grid(row=2, column=0)

send = Button(root, text="Send", font=FONT_BOLD, bg=BG_GRAY,
              command=send).grid(row=2, column=1)

root.mainloop()
