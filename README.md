# Python-Jarvis-AI-Project
import pyttsx3 
 #pip install pyttsx3 SpeechRecognition pywhatkit wikipedia requests 
import datetime
import os
import random
import smtplib
import webbrowser
import pywhatkit as kit
import speech_recognition as sr
import wikipedia

import cv2 #opencv-python
from requests import get


engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
#print(voices[0].id)
engine.setProperty('voice', voices[0].id)

#TEXT TO SPEECH
def speak(audio):
    engine.say(audio)
    print(audio)
    engine.runAndWait()

#CONVERT VOICE TO TEXT
def takecommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        #audio = r.listen(source, timeout=5, phrase_time_limit=10)

        audio = r.listen(source, timeout=5, phrase_time_limit=10)
    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language = 'en-in')
        print(f"user said: {query}")

    except Exception as e:
        speak("Say that again please i did'nt get that.....")
        return"none"
    return query

#For Greeting wishes
def wish():
    hour = int(datetime.datetime.now().hour)

    if hour>=0 and hour<12:
        speak("GOOD MORNING")
    elif hour>=12 and hour<18:
        speak("GOOD AFTERNOON")
    else:
        speak("GOOD EVENING")
    speak("I Am Jarvis ,Please Tell Me How Can I Help You")

#TO SEND EMAIL
def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('vidhi07898@gmail.com','ugtu utlk orkv sbel')
    server.sendmail('vidhi07898@gmail.com', to, content)
    server.close()


if __name__ == "__main__":

    wish()
    if 1:

        query = takecommand().lower()

        #Logic building for tasks

        if "open notepad" in query:
            npath = "C:\\Windows\\system32\\notepad.exe"
            os.startfile(npath)

        elif"open command prompt" in query:
            os.system("start cmd")

        elif "open camera" in query:
            cap = cv2.VideoCapture(0)

            while True:
                ret, img = cap.read()
                cv2.imshow('webcam', img)
                k = cv2.waitKey(50)
                if k==27:
                    break;
            cap.release()
            cv2.destroyAllWindows()

        elif"play music" in query:
            music_dir = "C:\\Users\\vidhi\\Downloads\\My music"
            songs = os.listdir(music_dir)
            rd =random.choice(songs)
            os.startfile(os.path.join(music_dir, rd))

        elif"ip address" in query:
            ip = get('https://api.ipify.org').text
            speak(f"your IP address is {ip}")

        elif "wikipedia" in query:
            speak("Searching Wikipedia......")
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("according to wikipedia")
            speak(results)
            #print(results)

        elif "open youtube" in query:
            webbrowser.open("www.youtube.com")

        elif "open facebook" in query:
            webbrowser.open("www.facebook.com")

        elif "open stackoverflow" in query:
            webbrowser.open("www.stackoverflow.com")

        elif "open instagram" in query:
            webbrowser.open("www.instagram.com")

        elif "open whatsapp" in query:
            webbrowser.open("https://web.whatsapp.com/")

        elif "open google" in query:
            webbrowser.open("www.google.com")

        elif "open gmail" in query:
            webbrowser.open("https://mail.google.com/mail/u/0/?tab=rm&ogbl#inbox")

        elif "whatsapp" in query:
            kit.sendwhatmsg("+916284547660", "Hey How Are You? ", 20, 38)

        elif "time" in query:
            strTime = datetime.datetime.now().strftime("%H:%M")
            speak(f"Sir, the time is {strTime}")

        elif "play songs on youtube" in query:
            kit.playonyt("see you again")



        elif "send email to vidhi" in query:
            try:
                speak("What should I say?")
                content = takecommand()
                to = "vidhich07@gmail.com"
                sendEmail(to, content)
                speak("Email has been sent to vidhi")

            except Exception as e:
                print(e)
                speak("sorry sir ,i am not able to sent it !")











