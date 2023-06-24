# Voiced-Based-Mail-For-Visually-Impaired-
import speech_recognition as sr
import smtplib
# import pyaudio
# import platform
# import sys
from bs4 import BeautifulSoup
import email
import imaplib
import pyttsx3 
import pyglet
import os, time

#pyglet.lib.load_library('avbin')
#pyglet.have_avbin=True

#project: :. Project: Voice based Email for blind :.

engine = pyttsx3.init()
"""VOICE"""
voices = engine.getProperty('voices')       #getting details of current voice
#engine.setProperty('voice', voices[0].id)  #changing index, changes voices. o for male or 1 for female
engine.setProperty('voice', voices[1].id) 
engine.say("Project: Voice-based Email for blind")
engine.save_to_file("Project: Voice-based Email for blind", "name.wav")
engine.runAndWait()

#os.system("name.wav")



#login from os
login = os.getlogin
print ("You are logging from : "+login())


# Initialize the engine
engine = pyttsx3.init()

# Choices
print("1. Compose a mail.")
engine.say("Option 1. Compose a mail.")
engine.runAndWait()

print("2. Check your inbox")
engine.say("Option 2. Check your inbox")
engine.runAndWait()

# Input choice
print("Your choice:")
engine.say("Your choice:")
engine.runAndWait()

# Convert text to speech and save to file
ttsname = "choice.mp3"
engine.save_to_file("Your choice", ttsname)
engine.runAndWait()

# Play the audio file
music = pyglet.media.load(ttsname, streaming=False)
music.play()

time.sleep(music.duration)
os.remove(ttsname)

#voice recognition part
r = sr.Recognizer()
with sr.Microphone() as source:
    
    audio=r.listen(source)
    print ("ok done!!")
    engine.say("ok done!!")
    engine.runAndWait()

try:
    text=r.recognize_google(audio)
    print ("You said : "+text)
    
except sr.UnknownValueError:
    print("Google Speech Recognition could not understand audio.")
    engine.say("Google Speech Recognition could not understand audio.")
    engine.runAndWait()
     
except sr.RequestError as e:
    print("Could not request results from Google Speech Recognition service; {0}".format(e)) 
    engine.say("Could not request results from Google Speech Recognition service")
    engine.runAndWait()

#choices details
if text == '1' or text == 'One' or text == 'one':
    r = sr.Recognizer() #recognize
    with sr.Microphone() as source:
        print ("Your message :")
        engine.say("Your message:")
        engine.runAndWait()
        audio=r.listen(source)
        print ("ok done!!")
        engine.say("ok done!!")
        engine.runAndWait()
    try:
        text1=r.recognize_google(audio)
        print ("You said : "+text1)
        engine.say("You said : "+text1)
        engine.runAndWait()
        msg = text1
    except sr.UnknownValueError:
        print("Google Speech Recognition could not understand audio.")
        engine.say("Google Speech Recognition could not understand audio.")
        engine.runAndWait()
    except sr.RequestError as e:
        print("Could not request results from Google Speech Recognition service; {0}".format(e))    

    mail = smtplib.SMTP('smtp.gmail.com', 587)    #host and port area
    mail.ehlo()  #Hostname to send for this command defaults to the FQDN of the local host.
    mail.starttls() #security connection
    mail.login('jaandwi08@gmail.com','typlfzfgsnelzdhi') #login part
    mail.sendmail('jaandwi08@gmail.com','janhavidwi7071@gmail.com',msg) #send part
    print ("Congrates! Your mail has send. ")
    engine.say("Congrates! Your mail has send. ")
    engine.runAndWait()
    engine = pyttsx3.init()
    tts = pyttsx3.init()
    tts.save_to_file("mail is:"+str(text1),"send.mp3")
    tts.runAndWait()
    music = pyglet.media.load("send.mp3", streaming = False)
    music.play()
    time.sleep(music.duration)
    os.remove("send.mp3")
    mail.close()   
    
if text == '2' or text == 'tu' or text == 'two' or text == 'Tu' or text == 'to' or text == 'To' :
    mail = imaplib.IMAP4_SSL('imap.gmail.com',993) #this is host and port area.... ssl security
    unm = ('your mail or victim mail')  #username
    psw = ('pswrd')  #password
    mail.login('jaandwi08@gmail.com','typlfzfgsnelzdhi')  #login
    stat, total = mail.select('Inbox')  #total number of mails in inbox
    print ("Number of mails in your inbox :"+str(total))
    engine.say("Number of mails in your inbox :"+str(total))
    engine.runAndWait()
    engine = pyttsx3.init()
    tts = pyttsx3.init()
    tts.save_to_file("Total mails are: " + str(total), "total.mp3")
    tts.runAndWait()
    music = pyglet.media.load("total.mp3", streaming=False)
    music.play()
    time.sleep(music.duration)
    os.remove("total.mp3")
    
    #unseen mails
    unseen = mail.search(None, 'UnSeen') # unseen count
    print ("Number of UnSeen mails :"+str(unseen))
    engine.say("Number of UnSeen mails :"+str(unseen))
    engine.runAndWait()
    engine = pyttsx3.init()
    tts = pyttsx3(text="Your Unseen mail :"+str(unseen), lang='en')
    ttsname=("unseen.mp3") #Example: path -> C:\Users\sayak\Desktop> just change with your desktop directory. Don't use my directory.
    tts.save("unseen.mp3")
    music = pyglet.media.load("unseen.mp3", streaming = False)
    music.play()
    time.sleep(music.duration)
    os.remove("unseen.mp3")
    
    #search mails
    result, data = mail.uid('search',None, "ALL")
    inbox_item_list = data[0].split()
    new = inbox_item_list[-1]
    old = inbox_item_list[0]
    result2, email_data = mail.uid('fetch', new, '(RFC822)') #fetch
    raw_email = email_data[0][1].decode("utf-8") #decode
    email_message = email.message_from_string(raw_email)
    print ("From: "+email_message['From'])
    print ("Subject: "+str(email_message['Subject']))
    engine = pyttsx3.init()
    tts = pyttsx3(text="From: "+email_message['From']+" And Your subject: "+str(email_message['Subject']), lang='en')
    ttsname=("mail.mp3") #Example: path -> C:\Users\sayak\Desktop> just change with your desktop directory. Don't use my directory.
    tts.save(ttsname)
    music = pyglet.media.load("mail.mp3", streaming = False)
    music.play()
    time.sleep(music.duration)
    os.remove("mail.mp3")
    
    #Body part of mails
    stat, total1 = mail.select('Inbox')
    stat, data1 = mail.fetch(total1[0], "(UID BODY[TEXT])")
    msg = data1[0][1]
    soup = BeautifulSoup(msg, "html.parser")
    txt = soup.get_text()
    print ("Body :"+txt)
    engine = pyttsx3.init()
    tts = pyttsx3(text="Body: "+txt, lang='en')
    ttsname=("body.mp3") #Example: path -> C:\Users\sayak\Desktop> just change with your desktop directory. Don't use my directory.
    tts.save(ttsname)
    music = pyglet.media.load("body.mp3", streaming = False)
    music.play()
    time.sleep(music.duration)
    os.remove("body.mp3")
    mail.close()
    mail.logout()
