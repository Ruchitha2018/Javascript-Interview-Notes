# Web Speech API

The **Web Speech API** is a browser API that allows web applications to work with **speech**.

It mainly has two parts:

1. **Speech Recognition** → Speech → Text
2. **Speech Synthesis** → Text → Speech

```text
             Web Speech API
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
Speech Recognition    Speech Synthesis
   Speech → Text         Text → Speech
```
---
### 1. Speech Recognition

Converts the user's voice into text.

```js
const recognition = new SpeechRecognition();

recognition.onresult = (event) => {
  const text = event.results[0][0].transcript;
  console.log(text);
};

recognition.start();
```

If the user says:

> Hello JavaScript

You might get:

```text
"Hello JavaScript"
```

Useful for:

* Voice search
* Voice commands
* Dictation
* Accessibility

---

### 2. Speech Synthesis

Converts text into spoken audio.

```js
const speech = new SpeechSynthesisUtterance(
  "Hello, welcome to JavaScript"
);

speechSynthesis.speak(speech);
```

The browser speaks the text aloud.

You can configure it:

```js
const speech = new SpeechSynthesisUtterance("Hello");

speech.lang = "en-US";
speech.rate = 1;
speech.pitch = 1;
speech.volume = 1;

speechSynthesis.speak(speech);
```
---
### Common APIs

| API                           | Purpose                    |
| ----------------------------- | -------------------------- |
| `SpeechRecognition`           | Converts speech to text    |
| `SpeechRecognition.start()`   | Starts recognition         |
| `SpeechRecognition.stop()`    | Stops recognition          |
| `SpeechRecognition.onresult`  | Receives recognized speech |
| `SpeechSynthesis`             | Controls text-to-speech    |
| `SpeechSynthesisUtterance`    | Represents text to speak   |
| `speechSynthesis.speak()`     | Speaks text                |
| `speechSynthesis.cancel()`    | Stops/removes speech       |
| `speechSynthesis.pause()`     | Pauses speech              |
| `speechSynthesis.resume()`    | Resumes speech             |
| `speechSynthesis.getVoices()` | Gets available voices      |

### Interview point

> **Web Speech API provides browser capabilities for speech recognition and speech synthesis, allowing applications to convert speech to text and text to speech.**


