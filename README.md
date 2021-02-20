import pyttsx3 #pip install pyttsx3
import speech_recognition as sr #pip install speechRecognition
import datetime
import wikipedia #pip install wikipedia
import webbrowser
import os
import smtplib



# initialize speech engine
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
 #print(voices[1].id)
engine.setProperty('voice', voices[1].id)

# text to speech function
def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Good Morning Sir!!")

    elif hour>=12 and hour<18:
        speak("Good Afternoon Sir!!")   

    else:
        speak("Good Evening Sir!!")  

    speak("I am narxi Sir.I am ur assitant, Please tell me how may I help you")       

# speech to text function
def takeCommand():
    #It takes microphone input from the user and returns string output

    r = sr.Recognizer()
    with sr.Microphone() as source:
        print('Listening...')
        r.pause_threshold = 1
        # wait for a second to let the recognizer adjust the
        # energy threshold based on the surrounding noise level
        r.adjust_for_ambient_noise(
            source, duration=1)
        # listens for the user's input
        audio = r.listen(source, timeout=3, phrase_time_limit=6)
        print('Processing audio now')

    try:
        print("Recognizing...")    
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")

    except Exception as e:
        print(e)    
        print("Say that again please...")  
        return "None"
    return query

def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    #identify to server
    server.ehlo()
    #encrypt session
    server.starttls()
    #login
    server.login('your email here', 'your password here')
    #send message
    server.sendmail('your email here ', 'recievers Mail here', content)
    #end mail connection
    server.close()

if __name__ == "__main__":
     wishMe()
     while True:
    # if 1:
        query = takeCommand().lower()

        # Logic for executing tasks based on query
        if 'wikipedia' in query:
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=4)
            speak("According to Wikipedia")
            print(results)
            speak(results)

        elif 'open youtube' in query:
            webbrowser.open("youtube.com")

        elif 'open google' in query:
            webbrowser.open("google.com")

        elif 'open stackoverflow' in query:
            webbrowser.open("stackoverflow.com")   


        elif 'play music' in query:
            music_dir = 'D:\\SONGS'
            songs = os.listdir(music_dir)
            print(songs)    
            os.startfile(os.path.join(music_dir, songs[1]))

        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")    
            speak(f"Sir, the time is {strTime}")

        elif 'open code' in query:
            codePath = "C:\\Users\\harsh\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
            os.startfile(codePath)

        elif 'email to harsh' in query:
            try:
                speak("What should I say?")
                content = takeCommand()
                to = "gargh4629@gmail.com"    
                sendEmail(to, content)
                speak("Email has been sent!!!")
            except Exception as e:
                print(e)
                speak("Sorry my friend harsh bhai. I am not able to send this email")  

        elif 'how are you' in query:
            speak("I am fine, Thank you")
            speak("How are you, Sir")
 
        elif 'fine' in query or "good" in query:
            speak("It's good to know that your fine")
 
        elif "change my name to" in query:
            query = query.replace("change my name to", "")
            assname = query
 
        elif "change name" in query:
            speak("What would you like to call me, Sir ")
            assname = takeCommand()
            speak("Thanks for naming me")
 
        elif "what's your name" in query or "What is your name" in query:
            speak("My friends call me")
            speak(assname)
            print("My friends call me", assname)
 
        elif 'exit' in query:
            speak("Thanks for giving me your time")
            exit()  


       
      

                
