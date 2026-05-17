<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0a0e1a,40:0d1f3c,70:1a3a6e,100:0a0e1a&height=220&section=header&text=HomeVox&fontSize=76&fontColor=ffffff&fontAlignY=38&fontStyle=bold&desc=Speak.%20It%20Listens.%20Your%20Home%20Responds.&descSize=18&descAlignY=62&descColor=7eb8f7" width="100%"/>

<br/>

<p align="center">
  <img src="https://img.shields.io/badge/AIML-Rule--Based%20NLP-1a3a6e?style=for-the-badge&logo=chatbot&logoColor=white"/>
  <img src="https://img.shields.io/badge/Google%20STT-Speech%20Recognition-4285F4?style=for-the-badge&logo=google&logoColor=white"/>
  <img src="https://img.shields.io/badge/pyttsx3-Text%20to%20Speech-FF6B35?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Input-Voice%20%7C%20Text-1a3a6e?style=flat-square"/>
  <img src="https://img.shields.io/badge/NLP-AIML%20Pattern%20Matching-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Sessions-Multi--User%20Support-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Interface-CLI%20%2B%20Interactive-orange?style=flat-square"/>
</p>

<br/>

> **HomeVox** is a voice-controlled smart home assistant powered by an AIML (Artificial Intelligence Markup Language) brain, Google Speech Recognition for speech-to-text, and pyttsx3 for text-to-speech output. Speak a command or type it — HomeVox understands, responds vocally, and acts. No internet dependency for the core AI engine.

<br/>

**[🧠 How It Works](#-how-it-works) · [⚙️ Architecture](#️-system-architecture) · [🚀 Quickstart](#-quickstart) · [📁 File Reference](#-file-reference) · [🔧 Configuration](#-configuration)**

</div>

---

## 🧠 How It Works

HomeVox combines three distinct technologies into one conversational pipeline:

### 1 — AIML Brain (Rule-Based NLP)

The intelligence layer is built on **AIML (Artificial Intelligence Markup Language)** — the same technology that powered ALICE, one of the earliest conversational AI systems. AIML defines pattern-response pairs that map user input to bot actions. All `.aiml` files live in the `aimlfiles/` folder and are compiled into a binary brain file (`bot_brain.brn`) for fast loading at runtime.

```xml
<!-- Example AIML pattern -->
<category>
  <pattern>TURN ON THE LIGHTS</pattern>
  <template>Turning on the lights for you.</template>
</category>
```

### 2 — Speech-to-Text (Google Speech Recognition)

Voice input is captured via your microphone using `SpeechRecognition` and sent to Google's Web Speech API for transcription. The returned text is fed directly into the AIML kernel as if typed.

```python
audio = recognizer.listen(source)
text  = recognizer.recognize_google(audio)   # STT
bot_response = kernel.respond(text)           # AIML brain
```

### 3 — Text-to-Speech (pyttsx3)

Every response from the AIML kernel is spoken aloud using `pyttsx3`, an offline text-to-speech engine. No internet needed for the voice output — it runs entirely on your local system audio.

---

## ⚙️ System Architecture

```
User (Voice or Text)
        │
        │  Microphone or keyboard
        ▼
  ┌─────────────────────────────────────────┐
  │           INPUT HANDLER                  │
  │                                          │
  │  Mode 1 → Text input (keyboard)          │
  │  Mode 2 → Voice input (microphone)       │
  │           SpeechRecognition + Google STT │
  └────────────────┬────────────────────────┘
                   │  Transcribed text
                   ▼
  ┌─────────────────────────────────────────┐
  │            AIML KERNEL                   │
  │                                          │
  │  Loads bot_brain.brn at startup          │
  │  Pattern matches user input              │
  │  Returns templated response              │
  │  Session-aware (multi-user support)      │
  └────────────────┬────────────────────────┘
                   │  Response text
                   ▼
  ┌─────────────────────────────────────────┐
  │          OUTPUT HANDLER                  │
  │                                          │
  │  Prints response to console              │
  │  pyttsx3 → speaks response aloud         │
  │  History logged to history/ folder       │
  └─────────────────────────────────────────┘
```

---

## ✨ Features

| Feature | Detail |
|---|---|
| 🎤 **Voice Input** | Captures spoken commands via microphone using Google Speech Recognition |
| ⌨️ **Text Input** | Fallback text mode for environments without a microphone |
| 🔊 **Voice Output** | Every response is spoken aloud via pyttsx3 offline TTS engine |
| 🧠 **AIML Brain** | Rule-based NLP engine — fast, deterministic, no model download required |
| 🗂️ **Session Management** | Each user gets a unique session ID — multi-user conversations stay separate |
| 📜 **Conversation History** | All sessions logged to the `history/` folder for review |
| ⚙️ **CLI Interface** | Full-featured command line tool with `help`, `usage`, verbose modes, and env config |
| 🔧 **Configurable** | Brain file path, session ID, verbosity, and default request all configurable via `.env` |

---

## 📁 File Reference

| File | Purpose |
|---|---|
| `main.py` | Interactive mode — choose text or voice input, continuous conversation loop |
| `executable-script.py` | CLI mode — pass session ID and request as arguments, scriptable and pipeable |
| `importable.py` | Module mode — import HomeVox functions into other Python scripts |
| `save-brain.ipynb` | Jupyter notebook to compile all `.aiml` files into `bot_brain.brn` |
| `1-save-brain.ipynb` | Alternative brain compilation notebook (incremental version) |
| `aimlfiles/` | All AIML rule files defining the smart home command patterns |
| `bot_brain.brn` | Compiled binary brain file — loaded at runtime for fast startup |
| `bot_brain-old.brn` | Previous brain version kept as backup |
| `history/` | Session conversation logs |

---

## 🚀 Quickstart

### 1. Clone the repository

```bash
git clone https://github.com/mahmedmajeedai/Voice-Controlled-Smart-Home-System.git
cd Voice-Controlled-Smart-Home-System
```

### 2. Install dependencies

```bash
pip install aiml SpeechRecognition pyttsx3 pyaudio python-dotenv
```

> On Windows, install PyAudio via wheel if `pip install pyaudio` fails:
> `pip install pipwin && pipwin install pyaudio`

### 3. Configure environment

```bash
cp .env.example .env
```

Edit `.env`:

```env
BRAIN_FILE=bot_brain.brn
VERBOSE=0
DEFAULT_REQUEST=Hi
STATIC_SESSION_ID=
```

### 4. Compile the AIML brain (first time only)

Open and run `save-brain.ipynb` in Jupyter, or run:

```bash
python -c "
import aiml, os
k = aiml.Kernel()
for f in os.listdir('aimlfiles'):
    if f.endswith('.aiml'): k.learn('aimlfiles/'+f)
k.saveBrain('bot_brain.brn')
print('Brain saved.')
"
```

### 5. Run in interactive mode

```bash
python main.py
```

Choose `1` for text input or `2` for voice input when prompted.

### 6. Run as a CLI command

```bash
# Single request with session ID
python executable-script.py my_session "turn on the lights"

# With verbose output
VERBOSE=1 python executable-script.py my_session "good morning"

# Show help
python executable-script.py help
```

---

## 🔧 Configuration

All settings are controlled via the `.env` file or environment variables:

| Variable | Default | Description |
|---|---|---|
| `BRAIN_FILE` | `bot_brain.brn` | Path to the compiled AIML brain file |
| `VERBOSE` | `0` | `0` = silent, `1` = show brain loading, `2` = show request and response |
| `DEFAULT_REQUEST` | `Hi` | Message sent when no request is provided via CLI |
| `STATIC_SESSION_ID` | *(empty)* | Override session ID — useful for single-user deployments |

---

## 🏠 Adding Smart Home Commands

All commands are defined in `.aiml` files inside the `aimlfiles/` folder. Add a new command by creating a pattern-response pair:

```xml
<aiml version="1.0">
  <category>
    <pattern>TURN ON THE FAN</pattern>
    <template>Turning on the fan. Enjoy the cool breeze!</template>
  </category>

  <category>
    <pattern>WHAT IS THE TEMPERATURE</pattern>
    <template>The current temperature is 22 degrees Celsius.</template>
  </category>
</aiml>
```

After adding new `.aiml` files, recompile the brain by running `save-brain.ipynb`.

---

## 🌍 Real-World Applications

| Domain | Use Case |
|---|---|
| 🏠 **Home Automation** | Control lights, fans, AC, and appliances via voice commands |
| ♿ **Accessibility** | Hands-free home control for users with mobility limitations |
| 🧓 **Elderly Care** | Simple voice-driven interface for smart home devices |
| 🎓 **NLP Education** | Hands-on demonstration of rule-based conversational AI with AIML |
| 🤖 **IoT Integration** | Extend the AIML responses to trigger GPIO or smart home APIs |

---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

<div align="center">

**Built by [Muhammad Ahmed Majeed](https://github.com/mahmedmajeedai)**

*Voice-first smart home automation powered by AIML + Google STT + pyttsx3*

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1a3a6e,50:0d1f3c,100:0a0e1a&height=120&section=footer" width="100%"/>

</div>
