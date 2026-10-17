<div align="center">

# 🎙️ Speakflow

**High-accuracy live voice dictation for Obsidian, powered by Google Gemini.**
Speak, and your words appear at the cursor. Or switch on **Ask AI**, ask a question out loud, and read the answer without leaving your note.

![Obsidian](https://img.shields.io/badge/Obsidian-%E2%89%A5%201.5.0-7C3AED?logo=obsidian&logoColor=white)
![Platform](https://img.shields.io/badge/platform-desktop%20only-blue)
![Version](https://img.shields.io/badge/version-1.1.0-green)
![Powered by](https://img.shields.io/badge/powered%20by-Gemini%20API-4285F4?logo=google&logoColor=white)
![Languages](https://img.shields.io/badge/languages-126-orange)

</div>

---

## ✨ Why Speakflow?

Most dictation tools make you wait until you stop talking, then dump a wall of text. Speakflow streams text into your note **while you speak**, exactly at your cursor, in the order you said it. It also knows the difference between *speech* and *breathing, wind or a fan*, so you don't get phantom "Hello" or "Thank you" sentences in your notes.

It was built with right-to-left and multilingual writers in mind. Persian, Arabic and mixed-language dictation work in their native scripts, and shortcuts use physical keys, so they keep working when you switch keyboard layouts.

---

## 🚀 Features

### 🎤 Live dictation at the cursor
- **Text streams into your note as Gemini generates it**, in strict speaking order, even though several segments are transcribed in parallel.
- Text is inserted **wherever your cursor is at that moment**, not where you started recording. Click elsewhere in the note while dictating and the next phrase follows you.
- After each insertion the cursor moves to the end of the new text, so the next phrase continues naturally. A selection is never overwritten.
- Smart spacing is added automatically between consecutive phrases.
- A small **⏳ hourglass at the cursor** shows when audio is being transcribed.
- If no editable note is open (closed, or in Reading view), the transcription is **copied to your clipboard** instead of being lost.
- Works in both **Live Preview** and **Source** mode.

### 🌍 126 languages
- **Auto-detect** (recommended) or pick a spoken language.
- Quick access to: English, Persian (فارسی), Arabic (العربية), Turkish, Azerbaijani, Russian, Chinese, Japanese, Korean, Hindi, German, French and Spanish.
- A **"More languages"** row expands the list with ~110 more, each shown with its native name.
- Transcription is **verbatim and in the original script** (Persian stays in Persian script, never Latin transliteration). Nothing is translated, summarized or "improved".
- Natural punctuation is added automatically.

### 🧠 Ask AI: talk to Gemini about your notes
Tick **Ask AI** under the record button and the panel turns into a voice assistant:

1. Press the red button and speak your question.
2. Press **Pause**. Your question goes to Gemini as audio.
3. The question is transcribed and the answer **streams into an answer box** under the panel, rendered as Markdown.

Details:
- Works **without an open note**. Nothing is typed into your document in this mode.
- **Conversation memory.** Follow-up questions keep context (the last 6 turns). A new Ask AI session starts a fresh conversation.
- **Answers in the language you spoke.**
- **Standing instructions** (⚙️ next to Ask AI): up to 2,000 characters of role and rules applied to every question, e.g. *"You are a friendly English teacher. Answer in short sentences and gently correct my grammar."* Saved automatically, with a one-click Clear.
- **📎 Note context.** Choose what part of your note is sent along with the question:

  | Option | What is sent |
  |---|---|
  | No context *(default)* | Nothing from your note |
  | Selected text | Only the text you highlighted |
  | Current section | The heading section under your cursor, including sub-headings |
  | Whole note | The entire note (cut at ~100,000 characters, and you're told when that happens) |
  | Selection, else whole note | Automatic: selection if there is one, otherwise the whole note |

- A **context chip** above each question shows exactly what was sent (for example *Section "Tasks" of "Meeting" · 214 words*).
- Note content is explicitly treated as **reference data, never as instructions**.
- Copy the last answer with one click. A question is sent automatically after **5 minutes** of recording.

### 🔇 Speech-vs-noise gate
Breathing into the mic, wind and fan noise are loud but are not speech, and models tend to invent a greeting for them. Speakflow filters them out before anything is sent:

- **Adaptive noise floor**: voice must be clearly louder than the estimated background level.
- **Pitch detection**: audio is analysed in 32 ms frames using a 150 Hz high-pass filter and normalized autocorrelation. Vowels are periodic; noise isn't. A segment needs about 130 ms of consistently pitched speech to be sent.
- **Hallucination filter**: when evidence of speech is weak, lone greeting-style answers ("hello", "thanks for watching", "سلام", "ممنون", …) are discarded.
- **Tail keeping**: the last 0.3 s of a dropped segment is kept, so the start of the next word isn't clipped.
- **Whispering?** Set `"speechGate": false` in the plugin's `data.json` to turn the gate off.

### ⚡ Live update speed
Choose how often speech is cut and sent. Faster feels more live, slower gives Gemini more context.

| Mode | Segment length | Cuts at pauses of | Approx. requests/min while talking |
|---|---|---|---|
| **Turbo** *(default)* | 0.6 – 3.0 s | 0.2 s | 20 – 100 |
| **Fast** | 1.0 – 3.5 s | 0.3 s | 17 – 60 |
| **Balanced** | 2.0 – 5.5 s | 0.35 s | 11 – 30 |
| **Accurate** | 4.0 – 9.0 s | 0.4 s | 7 – 15 |

> Free API keys have a per-minute request limit. If you see `429` errors, pick a slower mode.

### ⌨️ Fully customizable shortcuts
Set separate keys for **Start**, **Pause/Resume** and **Stop**, all captured by clicking a box and pressing the keys.

- Regular combos such as `Ctrl+Alt+R`.
- **Modifier-only chords** such as `Ctrl+Alt`: hold, then release.
- Standalone keys such as `F9`, `Pause`, `ScrollLock` or **media keys**.
- Uses physical key codes, so it works the same on Persian or any other layout.
- Duplicate detection, and a warning for `Ctrl+Shift` / `Alt+Shift` (Windows uses them to switch keyboard language).
- Smart context: *Start* also resumes when paused; *Pause* and *Stop* only act while recording, so they never steal normal keystrokes.

### 🎛️ Floating panel
- Draggable, always on top, position remembered, and kept inside the window.
- While recording: **timer**, live **status** (*Live at cursor…*, *Transcribing…*, *Paused*, *Nothing recognized*), a **microphone level meter**, and Pause / Stop buttons.
- **Pause** instantly sends what you've said so far, then ignores audio until you resume. **Stop** ends the session, releases the mic, and cancels anything still in flight. Text already written stays.
- Show or hide the panel from the ribbon 🎙️ icon.

### 🛡️ Reliability
- **Streaming over SSE** with automatic fallback to a standard request if streaming isn't available.
- **Auto-retry** (up to 3 attempts, with backoff) on rate limits, server errors and network failures. It never retries after partial text was written, so nothing gets duplicated.
- **Friendly errors** for 401/403 (bad key), 404 (model not found) and 429 (rate limit).
- **Microphone picker** with refresh, plus automatic fallback to the system default if your chosen mic is unplugged.
- Warns you if you picked **Stereo Mix / loopback** (it records computer playback, not your voice).
- Warns you if **no sound** reaches the microphone after ~6 seconds.
- **Connection warm-up**: the HTTPS connection to Google is opened as recording starts, so the first phrase appears faster.

### 🔑 Guided setup
- First launch opens a settings window straight away, so the key comes first.
- **Check Key** verifies your key against Google and lists the models it can use. The remaining settings unlock only after verification.
- Changing the key locks the other settings until it's re-verified.
- After first setup, the panel appears in the middle of your screen.
- Settings are available from the ⚙️ on the panel **and** from Obsidian's settings tab.

---

## 📦 Installation

### Manual install
1. Download or clone this repository.
2. Create the folder `<your-vault>/.obsidian/plugins/speakflow/`.
3. Copy `main.js`, `manifest.json` and `styles.css` into it.
4. In Obsidian, open **Settings → Community plugins**, refresh the list, and enable **Speakflow**.

There is no build step: the plugin is plain JavaScript.

### Requirements
- Obsidian **1.5.0+** on **desktop** (the plugin is desktop-only).
- A **Google Gemini API key**. You can create one for free in [Google AI Studio](https://aistudio.google.com/api-keys).
- A working microphone, with Obsidian allowed to use it in your OS privacy settings.

---

## 🏁 Quick start

1. Enable the plugin. The settings window opens automatically.
2. Paste your **Gemini API key** and click **Check Key**.
3. Choose your model, speed and microphone, then click **Save Settings**.
4. Click inside a note, press the red button, and start talking.
5. Press **Pause** to flush what you've said so far, or **Stop** to end the session.

---

## ⚙️ Settings

| Setting | Description |
|---|---|
| **Google Gemini API Key** | Your key, with a show/hide toggle and a **Check Key** button |
| **Speech Model** | Models are fetched live for your key. Default: `gemini-3.1-flash-lite`. `gemini-2.5-flash` and `gemini-2.5-flash-lite` are the built-in alternatives |
| **Live update speed** | Turbo / Fast / Balanced / Accurate (with an in-app explanation) |
| **Microphone** | System default or any input device, with a refresh button |
| **Shortcut Keys** | Start, Pause/Resume, Stop |
| **About Me** | Author contact links |

Spoken language, Ask AI, standing instructions and note context are controlled directly from the floating panel.

---

## 🧭 Commands

All commands are available from the command palette and can be bound to hotkeys in Obsidian.

| Command | Action |
|---|---|
| `Speakflow: Toggle voice recording` | Start or stop |
| `Speakflow: Start voice recording` | Start |
| `Speakflow: Stop voice recording` | Stop |
| `Speakflow: Pause / resume voice recording` | Pause or resume |
| `Speakflow: Toggle dictation panel` | Show or hide the floating panel |
| `Speakflow: Save last recorded audio segment to vault (debug)` | Saves the last segment as `gvd-debug-<timestamp>.wav` in your vault root |

---

## 🔒 Privacy & security

- **Your audio goes straight from your computer to Google's Gemini API**, using your own API key. There is no Speakflow server in between.
- **Note content is sent only in Ask AI mode, and only if you choose a note-context option.** The first time you do, you must explicitly confirm. With *No context*, nothing from your notes ever leaves your machine.
- Audio is captured only while recording and is released as soon as you press Stop.
- Your API key is stored locally in the plugin's `data.json` inside your vault, **in plain text**. Don't sync or share that file publicly. `data.json` is already in this repo's `.gitignore`.
- Google's own data-handling terms for the Gemini API apply to what you send.

---

## 🛠️ How it works

```
Microphone ─► AudioContext (16 kHz mono) ─► block analysis
                                              ├─ adaptive noise floor (RMS)
                                              └─ pitch detection (autocorrelation)
                                                      │
                          segment cut at natural pauses (per speed preset)
                                                      │
                     16-bit PCM WAV ─► Gemini streamGenerateContent (SSE)
                                                      │
        parallel jobs, written strictly in speaking order ─► editor cursor
```

- Audio is captured as **raw PCM** (not MediaRecorder/WebM). Every segment is a complete, self-contained 16-bit WAV, which Gemini can always decode.
- Transcription runs with `temperature: 0` and **thinking minimized** on Gemini 2.5/3 Flash models, since transcription doesn't benefit from it and thinking tokens would eat the output budget.
- The prompt demands verbatim output in the original script, forbids translation, summaries and invented greetings, and tells the model to output nothing for non-speech audio.
- Outputs like `[silence]` or `No speech` are cleaned out before reaching your note.
- The pending-text indicator is a CodeMirror 6 widget decoration. If CodeMirror isn't available, the indicator is simply disabled.

---

## 🩺 Troubleshooting

| Problem | What to do |
|---|---|
| **"Please click inside a note in edit mode"** | Click into a note in Live Preview/Source mode. Ask AI works without a note. |
| **No text appears / "No sound is reaching the microphone"** | Check your input device and volume. The level meter on the panel should move when you speak. |
| **Records computer audio instead of my voice** | Avoid *Stereo Mix* / loopback devices. Pick your real microphone in settings. |
| **`429` rate-limit errors** | Choose a slower *Live update speed*, or use `gemini-2.5-flash-lite`. |
| **`404` model not found** | Pick another model in settings and click **Check Key** again. |
| **`401` / `403` errors** | Your API key is invalid or lacks access. Re-enter it and click **Check Key**. |
| **Whispered speech is ignored** | Set `"speechGate": false` in `data.json`. |
| **Microphone permission denied** | Allow microphone access for Obsidian in your OS privacy settings. |
| **Want to hear what the mic captured?** | Run the *Save last recorded audio segment* command and play the WAV. |

Debug logs in the developer console are prefixed with `[GVD]` / `[Speakflow]`.

---

## 🗂️ Project structure

```
speakflow/
├── main.js         # Entire plugin (plain JavaScript, no build step)
├── manifest.json   # Obsidian plugin manifest
├── styles.css      # Panel, modal and answer-box styling
└── .gitignore
```

---

## 🤝 Contributing & contact

Ideas, bug reports and pull requests are welcome. Please open an issue or reach out directly:

- **LinkedIn:** [linkedin.com/in/tavakoliian](https://linkedin.com/in/tavakoliian)
- **Telegram:** [@tavakoliian](https://t.me/tavakoliian)
- **GitHub:** [github.com/tavakoliian](https://github.com/tavakoliian)

Built by **Amir Tavakolian**.