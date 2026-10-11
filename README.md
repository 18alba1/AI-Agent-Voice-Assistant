# Ecco6 — Physical AI Voice Assistant

A physical AI voice assistant built with **Raspberry Pi 3, Python, LangChain, and OpenAI**. Ecco6 combines speech recognition, conversational AI, tool calling, and text-to-speech to turn natural-language requests into useful actions across connected services and devices.

[Short summary slides of project](https://docs.google.com/presentation/d/1e8XE_i-fxLqr18WpGRSSurxCv7XLjGnBbeYa6GX-6LQ/edit?usp=sharing)

## Overview

Ecco6 is designed to make everyday tasks accessible through voice interaction. The user activates the assistant by saying **"Hello"**, asks a question or gives a command, and receives a spoken response.

Rather than functioning only as a conversational chatbot, Ecco6 uses a LangChain agent that can select and execute tools to interact with external APIs and services. This allows it to retrieve information, manage productivity tasks, control connected lights, and work with Raspberry Pi timer functionality.

The project brings together **physical computing, speech processing, LLM-based agents, and real-world API integrations** in a single system.

## Features

### Voice interaction
- Wake-word activation using the phrase "Hello".
- Microphone input and voice recording.
- Speech-to-text transcription using OpenAI Whisper.
- AI-generated responses using OpenAI language models.
- Text-to-speech generation using OpenAI's audio API.
- Audio playback for spoken assistant responses.

### AI agent and tool calling
- LangChain agent that selects tools based on the user's request.
- Conversation history passed to the language model.
- Structured tool definitions for connecting natural-language requests to Python functions.
- Tool integrations for productivity, real-time information, travel, and connected devices.

### Productivity and connected services
- **Google Calendar:** Retrieve events and create, modify, or remove calendar events.
- **Gmail:** Retrieve unread messages and send emails.
- **Google Tasks:** Manage task lists and tasks, including creating and removing them.
- **Google Docs:** Create documents and insert text.
- **Weather and news:** Retrieve weather information and top news headlines.
- **Alarms:** Create, list, modify, and remove alarms.
- **Timers:** Set a countdown using the Raspberry Pi timer integration.
- **Smart lighting:** Turn lights on or off and adjust brightness.
- **Location and public transport:** Retrieve location-based information, find nearby stops, and get travel suggestions.

Some integrations require authentication, configuration, and access to the corresponding external service.

## How It Works

Ecco6 uses a multi-stage voice interaction pipeline.

```text
User speaks
    |
    v
Wake-word detection ("Hello")
    |
    v
Record the user's question
    |
    v
OpenAI Whisper speech-to-text
    |
    v
LangChain agent + OpenAI language model
    |
    v
Select and execute tools when needed
    |
    v
Generate a natural-language response
    |
    v
OpenAI text-to-speech
    |
    v
Play the spoken response
```

### 1. Wake-word detection

The application continuously listens for the wake word "Hello" using the Python SpeechRecognition library and a microphone. Google speech recognition is used to recognize the wake word.

When the wake word is detected, the application plays an audio confirmation and starts recording the user's request.

### 2. Speech-to-text

The recorded audio is sent to OpenAI's Whisper API using the `whisper-1` model. The transcription is converted into text and added to the conversation history.

### 3. AI reasoning and tool selection

The transcribed request is passed to the Ecco6 agent. LangChain connects the OpenAI language model to a collection of structured Python tools.

Based on the request, the agent can either answer conversationally or select a tool to retrieve information or perform an action. For example, a request about upcoming calendar events can trigger the Google Calendar integration.

### 4. Tool execution

The selected Python function interacts with the relevant API, service, or connected device. The result is returned to the agent so it can formulate a response.

### 5. Text-to-speech

The response is sent to OpenAI's text-to-speech API using the `tts-1` model. The generated audio is then played back to the user. The voice can be configured in the application settings.

## Architecture and Technology Stack

| Component | Technology | Purpose |
|---|---|---|
| Physical computing | Raspberry Pi 3 | Device-oriented assistant and timer integration |
| Programming language | Python | Application logic and integrations |
| AI agent framework | LangChain | Tool calling and agent orchestration |
| Language model | OpenAI ChatGPT API | Conversational responses and tool selection |
| Speech-to-text | OpenAI Whisper API | Transcribes recorded speech |
| Wake-word recognition | SpeechRecognition, Google speech recognition | Detects the "Hello" activation phrase |
| Text-to-speech | OpenAI TTS API | Converts responses into audio |
| User interface | Streamlit | Application interface, settings, and service connections |
| Authentication | Google OAuth, Firebase Authentication | Access control and Google service authorization |
| External integrations | Google APIs and HTTP APIs | Productivity, information retrieval, and device control |

## Example Use Cases

| Voice request | Assistant capability |
|---|---|
| "What is the weather in Stockholm?" | Retrieve weather information |
| "What is on my calendar today?" | Retrieve calendar events |
| "Send an email..." | Use the Gmail integration |
| "Add a task to my list..." | Create a Google Task |
| "Set an alarm..." | Create an alarm |
| "Turn on the light." | Call the smart-light integration |
| "Find nearby public transport." | Retrieve nearby stops and travel information |

These examples illustrate the available tool categories; the exact result depends on the connected services, configuration, and arguments provided.

## Getting Started

### Prerequisites

- Python and pip.
- Git.
- An OpenAI API key.
- A Raspberry Pi 3 and compatible audio hardware for the physical setup.
- Credentials for any Google or other external services you want to use.

### 1. Clone the repository

```bash
git clone https://github.com/18alba1/AI-Agent-Voice-Assistant.git
cd AI-Agent-Voice-Assistant
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

On Windows, activate the environment with:

```powershell
venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure API keys and credentials

Configure the OpenAI API key and the credentials needed for the integrations you intend to use. The application accesses configuration through Streamlit secrets, including settings for OpenAI, Google authentication, and smart-light services.

Create the required secrets configuration based on the keys referenced by the application. Set up Google OAuth credentials and enable the relevant Google APIs before connecting a Google account.

**Security:** Never commit API keys, OAuth client secrets, access tokens, or other private credentials to the repository.

### 5. Launch the application

The current repository uses Streamlit as its application entry point:

```bash
streamlit run ecco6/main.py
```

For the physical setup, configure the Raspberry Pi's microphone, speaker or audio output, and any required device-specific services. The Raspberry Pi timer integration also requires the appropriate timer server URL.

> The commands above follow the repository's current Streamlit structure. Confirm the required secrets, audio-device configuration, and Raspberry Pi services before running the complete setup.

## Engineering Highlights

This project provided practical experience with:

- **LLM agents and function calling:** Connecting a language model to structured tools that perform real operations.
- **Voice interfaces:** Building a pipeline from wake-word detection and speech transcription to generated audio responses.
- **API integration:** Connecting Python functions to Google services, information APIs, and smart-device endpoints.
- **Authentication and configuration:** Working with OAuth and external service credentials.
- **Physical AI:** Combining language-model capabilities with a Raspberry Pi-based device.
- **Modular design:** Organizing integrations as separate tools that can be selected and executed by the agent.

## Future Improvements
- Add more robust error handling for API failures and network interruptions.
- Add confirmation steps for sensitive actions, such as sending emails or deleting tasks.
- Expand hardware integration and improve deployment documentation.
