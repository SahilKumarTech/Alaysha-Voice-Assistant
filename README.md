# Alaysha-Voice-Assistant
A simple AI voice assistant that listens for commands and performs tasks like opening websites or playing music**.
>>Features of This Voice Assistant (Alaysha)**
This Python voice assistant named Alaysha listens for voice commands and performs various tasks like opening websites and playing music.
>>1. Voice-Activated Wake Word ("Alaysha")

The assistant stays on standby and listens for the wake word "Alaysha.
If someone says "Alaysha," the assistant wakes up and asks "Yes, how can I help?"
If no wake word is detected, the assistant ignores the background noise and continues waiting.
You can sa
>>2. Opens Popular Websites Using Voice Commands
You can say:

"Open Google" → It will open Google.com in the browser.
"Open Facebook" → It will open Facebook.com in the browser.
"Open YouTube" → It will open YouTube.com in the browser.
"Open LinkedIn" → It will open LinkedIn.com in the browser. 

>>3. Plays Predefined Songs on YouTube:

If you say "Play Skyfall", it will open the YouTube link for the song "Skyfall."
If you say "Play March", it will open the March song on YouTube.
The assistant will confirm by saying "Playing Skyfall", etc.
The music list is predefined inside the music_library dictionary.

>>4. Converts Speech to Text

The program listens to your voice and converts it into text using speech_recognition.
It then compares the text with predefined commands and takes action.
If speech is unclear, Alaysha will say:

>>>>Final Thoughts:
This is a simple AI voice assistant that can listen, understand, and perform actions based on your commands. It's a great starting project for AI & Python automation. Let me know if you want to add more features!
.................................Code in given below...........................................
import speech_recognition as sr  # Library for speech recognition
import webbrowser  # Library to open web pages
import pyttsx3  # Library for text-to-speech conversion

# Music Library with predefined YouTube links
music_library = {
     # English Songs
    "stealth": "https://www.youtube.com/watch?v=U47Tr9BB_wE",
    "march": "https://www.youtube.com/watch?v=Xqeq4b5u_Xw",
    "skyfall": "https://www.youtube.com/watch?v=DeumyOzKqgI",
    "wolf": "https://www.youtube.com/watch?v=ThCH0U6aJpU",
    
    # Hindi Songs
    "tum hi ho": "https://www.youtube.com/watch?v=Umqb9KENgmk",  # Tum Hi Ho - Aashiqui 2
    "kesariya": "https://www.youtube.com/watch?v=BddP6PYo2gs",  # Kesariya - Brahmāstra
    "tera ban jaunga": "https://www.youtube.com/watch?v=HGQh_WCz4Rw",  # Tera Ban Jaunga - Kabir Singh
    "dil diya gallan": "https://www.youtube.com/watch?v=mmKuHX8n-rw",  # Dil Diyan Gallan - Tiger Zinda Hai
    "kaun tujhe": "https://www.youtube.com/watch?v=NotgaKxHnMo",  # Kaun Tujhe - M.S. Dhoni
    "shayad": "https://www.youtube.com/watch?v=nfN9aR-jt7c",  # Shayad - Love Aaj Kal
    "bekhayali": "https://www.youtube.com/watch?v=1jH3pJZB4Jw",  # Bekhayali - Kabir Singh
    "channa mereya": "https://www.youtube.com/watch?v=284Ov7ysmfA",  # Channa Mereya - Ae Dil Hai Mushkil
    "kal ho naa ho": "https://www.youtube.com/watch?v=HJbHvx_5EEo",  # Kal Ho Naa Ho - Title Track
    "agar tum saath ho": "https://www.youtube.com/watch?v=sK7riqg2mr4"  # Agar Tum Saath Ho
}

# Initialize speech recognizer and text-to-speech engine
recognizer = sr.Recognizer()
engine = pyttsx3.init()

def speak(text):
    """This function converts text into speech and speaks it."""
    engine.say(text)
    engine.runAndWait()

def process_command(command):
     
    command = command.lower()  # Convert the command to lowercase for easy comparison
    
    # Check if the command matches a known action
    if "open google" in command:
        webbrowser.open("https://google.com")
        speak("Opening Google")
    elif "open facebook" in command:
        webbrowser.open("https://facebook.com")
        speak("Opening Facebook")
    elif "open youtube" in command:
        webbrowser.open("https://youtube.com")
        speak("Opening YouTube")
    elif "open linkedin" in command:
        webbrowser.open("https://linkedin.com")
        speak("Opening LinkedIn")
    
    # Check if the command is to play music
    elif command.startswith("play"):
        song_name = command[5:].strip()  # Remove "play " from the command to get the song name
        if song_name in music_library:
            webbrowser.open(music_library[song_name])  # Open the song in the web browser
            speak(f"Playing {song_name}")
        else:
            speak("Sorry, this song is not available in the library.")
    
    # If command is not recognized
    else:
        speak("I didn't understand that command.")

# Main program execution
if __name__ == "__main__":
    speak("Initializing Alaysha....")  # Assistant's startup message
    
    while True:
        r = sr.Recognizer()  # Create a new recognizer instance
        
        try:
            # Listen for the wake word "Alaysha"
            with sr.Microphone() as source:
                print("Listening for wake word 'Alaysha'...")
                r.adjust_for_ambient_noise(source)  # Adjust for background noise
                audio = r.listen(source, timeout=5, phrase_time_limit=3)  # Listen for speech
                word = r.recognize_google(audio).lower()  # Convert speech to text
            
            # If the wake word is detected, proceed to listen for a command
            if word == "alaysha":
                speak("Yes, how can I help?")
                
                with sr.Microphone() as source:
                    print("Alaysha is listening for your command...")
                    r.adjust_for_ambient_noise(source)  # Adjust for background noise
                    audio = r.listen(source, timeout=5, phrase_time_limit=5)  # Listen for command
                    command = r.recognize_google(audio)  # Convert speech to text

                    process_command(command)  # Process the recognized command
        
        except sr.UnknownValueError:
            print("Sorry, I couldn't understand. Please try again.")  # If speech is unclear
        except sr.RequestError:
            print("Network error. Please check your internet connection.")  # If there's an internet issue
        except Exception as e:
            print(f"Error: {e}")  
