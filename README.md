import pyttsx3 #pip install pyttsx3
import speech_recognition as sr #pip install speechRecognition
import wikipedia #pip install wikipedia
import datetime
import webbrowser
import os
import smtplib

engine = pyttsx3.init('sapi5') #pip install api
voices = engine.getProperty('voices')

engine.setProperty('voice', voices[0].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour >= 0 and hour < 12:
        speak("Good Morning Mr.Roy!")

    elif hour >= 12 and hour < 18:
        speak("Good Afternoon Mr.Roy!")

    else:
        speak("Good Evening Mr.Roy!")

    speak("I am Friday, your one and only artificial intelligence. How can I help you sir? Please order me.")


def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")

    except Exception as e:
        # print(e)
        print("Say that again please...")
        return "None"
    return query

def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('your@gmail.com', 'your password')
    server.sendmail('your@gmail.com', "reciever@gmail.com", content)
    server.close()

def getTime():
    now = datetime.datetime.now()
    meridiem = ' '
    if now.minute < 10:
        minute = 0+str(now.minute)
    else:
        minute = str(now.minute)
    if now.hour >= 12:
        meridiem = 'pm'
        hour = str(now.hour)
    else:
        meridiem = 'am'
        hour = str(now.hour)

    return 'The current time is '+hour+':'+minute+' '+meridiem+' .'

if __name__ == "__main__":

    wishMe()
    while True:

        query = takeCommand().lower()


        if 'according to wikipedia' in query:
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            print(results)
            speak(results)

        elif 'what is your name' in query:
            speak('my name is Friday')

        elif 'open youtube' in query:
            speak('Openning YouTube, please wait')
            url = "youtube.com"
            chrome_path = 'C:/Program Files (x86)/Google/Chrome/Application/chrome.exe %s'
            webbrowser.get(chrome_path).open(url)

        elif 'open google' in query:
            speak('Opening Google, Please wait.')
            url = "google.com"
            chrome_path = 'C:/Program Files (x86)/Google/Chrome/Application/chrome.exe %s'
            webbrowser.get(chrome_path).open(url)

        elif 'music' in query:
            speak('Let me play a music for your satisfaction')
            music_dir = 'C:\\Users\\Anurag\\Desktop\\FOR ALL FOLDERS\\Desktop Songs\\english musics'
            songs = os.listdir(music_dir)
            print(songs)
            os.startfile(os.path.join(music_dir, songs[0]))

        elif 'time' in query:
            print(getTime())
            speak(getTime())

        elif 'email to reciever's name' in query.lower():
            try:
                speak("What should I send?")
                content = takeCommand()
                to = "reciever@gmail.com"
                sendEmail(to, content)
                speak("Email has been sent!")
            except Exception as e:
                print(e)
                speak("Sorry my boss Mr. Roy. I am not able to send this email")

        elif 'goodbye friday' in query:
            speak('Good bye Mr.Roy. It was wonderful to serve you. Have a nice day.')
            exit()
