# Install necessary libraries
!pip install googletrans==4.0.0-rc1 gTTS ipywidgets

# Import required libraries
from gtts import gTTS
from googletrans import Translator
from IPython.display import Audio, display
import ipywidgets as widgets

# Read text from the input file (replace 'input.txt' with your file path)
input_file_path = '/content/kalki.txt'  # Upload your Kalki story text file to Colab

with open(input_file_path, 'r') as file:
    text = file.read()

# Create a dropdown to select the language for translation
language_options = {
    'Tamil': 'ta',
    'Telugu': 'te',
    'Kannada': 'kn',
    'Malayalam': 'ml',
    'Hindi': 'hi',
}

# Create a dropdown widget
language_dropdown = widgets.Dropdown(
    options=language_options,
    description='Language:',
    disabled=False
)

# Display the dropdown
display(language_dropdown)

# Function to handle translation and text-to-speech conversion
def translate_and_speak(b):
    try:
        # Translate text from English to selected language
        translator = Translator()
        translated = translator.translate(text, src='en', dest=language_dropdown.value)
        translated_text = translated.text  # Getting the translated text

        # Convert translated text to speech
        tts = gTTS(text=translated_text, lang=language_dropdown.value)
        tts.save("translated_output.mp3")
        
        # Play the translated speech as audio output
        display(Audio("translated_output.mp3", autoplay=True))  # Ensure autoplay is True to auto play audio

    except Exception as e:
        print(f"Error: {e}")

# Button to trigger translation and speech conversion
button = widgets.Button(description="Convert to Speech")
button.on_click(translate_and_speak)

# Display the button
display(button)