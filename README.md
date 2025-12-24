# voiceactivatedSOSsystem
SOS alert system
import subprocess
import speech_recognition as sr

#--------------------------------------------------------------------------------
# FUNCTION TO RECOGNIZE SPEECH AND RETURN RESPONSE
def recognize_speech_from_mic(recognizer, microphone):
    if not isinstance(recognizer, sr.Recognizer):
        raise TypeError("`recognizer` must be `Recognizer` instance")
    if not isinstance(microphone, sr.Microphone):
        raise TypeError("`microphone` must be `Microphone` instance")

    with microphone as source:
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)

    response = {
        "success": True,
        "error": None,
        "transcription": None
    }

    try:
        response["transcription"] = recognizer.recognize_google(audio)
    except sr.RequestError:
        response["success"] = False
        response["error"] = "API unavailable"
    except sr.UnknownValueError:
        response["error"] = "Unable to recognize speech"

    return response

#--------------------------------------------------------------------------------
# FUNCTION TO RUN WHATSAPP
def run_whatsapp():
    try:
        # WhatsApp Desktop path (adjust if installed elsewhere)
        subprocess.Popen(r"C:\Users\<YourUserName>\AppData\Local\WhatsApp\WhatsApp.exe")
        return "Success"
    except FileNotFoundError as e:
        return str(e)

#--------------------------------------------------------------------------------
# MAIN : Where all functions are called
if __name__ == "__main__":
    recognizer = sr.Recognizer()
    microphone = sr.Microphone()

    print("-- Speak Prompt --")

    guess = recognize_speech_from_mic(recognizer, microphone)
    if guess["transcription"]:
        print("You said: {}".format(guess["transcription"]))
    if not guess["success"]:
        print("Error with API")
    if guess["error"]:
        print("ERROR: {}".format(guess["error"]))

    if guess["transcription"]:
        spoken_text = guess["transcription"].lower()
        if "open" in spoken_text.split():
            print("Opening WhatsApp...")
            app_run_status = run_whatsapp()
            print("App Run Status:", app_run_status)
        else:
            print("Keyword 'open' not detected")import subprocess
import pyautogui
import time

# Open WhatsApp Desktop
subprocess.Popen(r"C:\Users\<YourUserName>\AppData\Local\WhatsApp\WhatsApp.exe")

# Wait for WhatsApp to load (adjust timing if needed)
time.sleep(5)

# Search for a contact (you must know the contact name)
pyautogui.hotkey("ctrl", "f")   # opens search bar
time.sleep(1)
pyautogui.typewrite("ContactName")  # replace with actual contact name
time.sleep(1)
pyautogui.press("enter")

# Type and send the message
pyautogui.typewrite("I need help")
pyautogui.press("enter")
