import speech_recognition as sr
import pyttsx3
import pywhatkit
import datetime
import wikipedia
import pyjokes
import tkinter as tk
import webbrowser
from threading import Thread


listener = sr.Recognizer()
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)

app = tk.Tk()
app.title("Serena Assistant")
app.configure(bg='black')  # Set the background color of the main window

text_var = tk.StringVar()
text_var.set("Welcome to Serena Assistant")

label = tk.Label(app, textvariable=text_var, font=('Helvetica', 16), bg='lightblue', fg='black')  # Set label colors
label.pack(pady=10)

message_var = tk.StringVar()
message_var.set("")

message_label = tk.Label(app, textvariable=message_var, font=('Helvetica', 12), wraplength=300, bg='lightyellow', fg='black')  # Set label colors
message_label.pack(pady=10)

button_start_stop_var = tk.StringVar()
button_start_stop_var.set("Start")

def talk(text):
    engine.say(text)
    engine.runAndWait()

def take_command():
    command = ""
    try:
        with sr.Microphone() as source:
            print('listening...')
            text_var.set("Listening...")
            app.update_idletasks()
            voice = listener.listen(source, timeout=5)
            command = listener.recognize_google(voice)
            command = command.lower()
            if 'hello' in command:
                talk('Hello')
                command = command.replace('serena', '')
                print(command)
    except sr.WaitTimeoutError:
        print("Speech recognition timed out. No speech detected.")
        text_var.set("Speech recognition timed out. No speech detected.")
    except sr.UnknownValueError:
        print("Speech recognition could not understand audio.")
        text_var.set("Speech recognition could not understand audio.")
    except sr.RequestError as e:
        print(f"Could not request results from Google Speech Recognition service; {e}")
        text_var.set("Could not request results from Google Speech Recognition service.")
    app.after(5000, take_command)  # Schedule the next execution after 5 seconds
    return command

def run_serena():
    command = take_command()
    print(command)
    if 'play' in command:
        query = command.replace('play', '').strip()
        talk('opening ' + query + ' on YouTube')
        url = 'https://www.youtube.com/results?search_query=' + '+'.join(query.split())
        webbrowser.open(url)
        message_var.set("Opening YouTube for: " + query)
        
    elif 'time' in command:
        current_time = datetime.datetime.now().strftime('%I:%M %p')
        talk('Current time is ' + current_time)
        message_var.set("Current time is: " + current_time)

    elif 'tell about' in command:
        topic = command.replace('tell about', '').strip()
        info = wikipedia.summary(topic, 1)
        print(info)
        talk(info)
        message_var.set("Information about " + topic + ": " + info)

    elif 'date' in command:
        talk('sorry, I have a headache')
        message_var.set("Sorry, I have a headache")

    elif 'are you single' in command:
        talk('I am in a relationship with wifi')
        message_var.set("I am in a relationship with wifi")

    elif 'joke' in command:
        joke = pyjokes.get_joke()
        talk(joke)
        message_var.set("Joke: " + joke)

    else:
        talk('I can not hear you.')
        print('I can not hear you.')
        message_var.set("I cannot hear you.")

def toggle_recognition():
    global gui_thread
    if button_start_stop_var.get() == "Start":
        gui_thread = Thread(target=update_label)
        gui_thread.start()
        button_start_stop_var.set("Stop")
    else:
        gui_thread.join()
        button_start_stop_var.set("Start")

button_start_stop = tk.Button(app, textvariable=button_start_stop_var, command=toggle_recognition, bg='lightgreen', fg='black')  # Set button colors
button_start_stop.pack(pady=10)

def update_label():
    run_serena()
    text_var.set("Welcome to Serena Assistant")
    app.update_idletasks()
    app.after(100, update_label)  # Schedule the next update after 100 milliseconds

app.mainloop()
