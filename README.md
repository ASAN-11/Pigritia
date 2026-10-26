# Pigritia

**Experience AI core but slow and steady.**

Pigritia is a small set of lazy-day helpers that run entirely on your own machine. It is a single HTML file that talks to [Ollama](https://ollama.com) and an open-weight [Gemma](https://ai.google.dev/gemma) model. There is no backend, no account, no build step and no API key. Your messages, photos and file names never leave your computer.

The name comes from the Latin *pigritia*, meaning sloth, in the laziness sense. It was built for the DEV "Slow and Steady" open-source AI challenge, with one rule: make the model do the work so you don't have to.

---

## Contents

- [What it does](#what-it-does)
- [Why open models](#why-open-models)
- [Requirements](#requirements)
- [Quick start](#quick-start)
- [The site, section by section](#the-site-section-by-section)
- [Using each tool](#using-each-tool)
- [How it works](#how-it-works)
- [Privacy and data handling](#privacy-and-data-handling)
- [Choosing a model](#choosing-a-model)
- [Troubleshooting](#troubleshooting)
- [Limitations](#limitations)
- [Customising](#customising)
- [Project structure](#project-structure)

---

## What it does

| Tool | You give it | You get back |
|---|---|---|
| **Autopilot** | An email thread, meeting notes or a group chat | A short summary, a priority tag, questions waiting on you, a task list with owners and due dates, a calendar file and a drafted reply |
| **Fridge list** | A photo of your fridge or pantry | A list of what the model can see and a tickable shopping checklist |
| **Wind down** | A story idea | A calm bedtime story, read aloud with your system voice, plus generated rain, ocean or forest ambience |
| **File tidier** | A list of messy file names | A rename-and-sort plan and a ready-to-run `.sh` or `.ps1` script |

Everything is produced by a model running through your local Ollama. The ambient sound and text-to-speech use your browser's built-in capabilities, so they also work offline.

---

## Why open models

Pigritia only works because the pieces are open.

- **Offline.** Once a model is pulled, nothing needs the internet. Run it on a train, on a plane or with the Wi-Fi switched off.
- **Private by construction.** Group chats, work email and photos of your kitchen are exactly the things you would not want to paste into someone else's server. Here they go to `localhost` and nowhere else.
- **Free to run.** There is no per-token bill. Set it up once and use it as much as you like.
- **Swappable.** Pigritia does not care which model answers. Change the model in Setup and the whole site uses it. Fine-tune your own, pick a smaller one for an old laptop, or a larger one when quality matters.
- **Inspectable.** The whole app is one readable file. The prompts that drive each tool are in plain sight and easy to change.

---

## Requirements

- [Ollama](https://ollama.com/download) installed and running
- A Gemma model pulled in Ollama, for example `gemma3`
- A modern desktop browser (Chrome, Edge, Firefox or Safari)
- For the **Fridge list** tool only: a vision-capable model, such as `gemma3:4b` or larger. Very small text-only variants cannot read images.

Hardware needs depend on the model you choose. Smaller models run on a typical laptop; larger ones want more RAM or a GPU. See the Ollama model pages for sizes.

---

## Quick start

1. **Install Ollama** from <https://ollama.com/download>.

2. **Pull a model**

   ```bash
   ollama pull gemma3
   ```

3. **Start Ollama with this page allowed to call it.** Quit any running Ollama first, then:

   macOS or Linux:

   ```bash
   OLLAMA_ORIGINS="*" ollama serve
   ```

   Windows (PowerShell):

   ```powershell
   $env:OLLAMA_ORIGINS="*"; ollama serve
   ```

   `OLLAMA_ORIGINS` tells Ollama which web pages may talk to it. A page opened straight from disk has no normal origin, so allowing it explicitly is the most reliable route. If you would rather not use `*`, serve the page over `http://localhost` (see below) and allow only that origin.

4. **Open the app.** Double-click `pigritia.html`, or serve it:

   ```bash
   python3 -m http.server 8000
   # then open http://localhost:8000/pigritia.html
   ```

5. **Check the sidebar.** The bottom-left status shows a green dot and the model name when Ollama is reachable. If it says "Ollama offline", go to **Setup** and follow the message there.

---

## The site, section by section

Pigritia has a vertical sidebar and six pages. The sidebar collapses to icons on narrow screens, and the page you were last on is remembered.

**Home.** A hero with the sloth mark, four tiles that jump to each tool, a three-column "how it works" strip (bring the mess, Gemma thinks locally, you just approve) and a short note on why open models matter here.

**Autopilot.** Triage for anything messy and text-based. Paste or load text, choose a reply tone and run it. Results appear as cards: summary and priority, pending questions, tasks, and an editable drafted reply.

**Fridge list.** Upload a photo, optionally say what you plan to cook, and get a checklist you can tick off at the shop.

**Wind down.** A story generator with length options, a read-aloud button, and an ambient sound player with a volume slider.

**File tidier.** Pick files or paste names, review a before and after table, then download a script.

**Setup.** Set the Ollama address, pick from the models Ollama reports as installed, and see connection status. First-time steps are listed on the page.

---

## Using each tool

### Autopilot

1. Open **Autopilot** in the sidebar.
2. Paste text into the box, or load a `.txt`, `.log`, `.md` or `.eml` file.
3. Choose a reply tone: *Friendly*, *Professional* or *Short and direct*.
4. Press **Run autopilot**. Use **Stop** to cancel.

You get:

- **Summary** with a coloured priority tag: urgent, normal or low.
- **Waiting on your answer**: the questions the text is asking you.
- **Tasks**: each with an owner and, if one was stated, a due date. Tick them off as you go.
- **Drafted reply**: written for you to send. It is an editable text box, so change anything before copying.

Buttons:

- **Download calendar (.ics)** creates all-day calendar events for tasks that have a due date. Import the file into Google Calendar, Apple Calendar or Outlook. Tasks without a date are skipped, and the page tells you if none have one.
- **Copy as checklist** puts a Markdown task list on your clipboard.
- **Copy reply** copies the drafted reply.

Relative dates such as "Friday" or "next week" are resolved using today's date, which Pigritia passes to the model. Only the last 24,000 characters of very long text are used.

### Fridge list

1. Open **Fridge list** and choose a photo.
2. Optionally type a plan, such as "pasta twice and a veggie curry".
3. Press **Make my list**.

The model first lists what it can see, then what to buy. The "buy" lines become a checklist with tick-boxes. Photos are shrunk to a maximum of 1024 pixels on the longest side before being sent to the local model, which keeps it fast. Clear, well-lit photos work best.

### Wind down

1. Type a story idea. The default is a sloth taking the long way to a mango tree.
2. Pick a length: short (about 250 words), medium (about 500) or long (about 800).
3. Press **Tell me a story**. The story streams in as it is written.
4. Press **Read aloud** to hear it, or **Stop reading** to silence it. Reading uses your browser's built-in voices at a slow pace.
5. In **Ambient sound**, pick rain, ocean or forest wind, press **Play** and set the volume. Changing the sound type while playing switches it immediately.

The story prompt asks for a slow, low-stakes story with no cliffhangers, ending with the character falling asleep. The sounds are synthesised in the browser with the Web Audio API, so there are no audio files and no downloads.

### File tidier

1. Choose files with the file picker, or paste file names one per line. Only the names are read; the files are not opened or uploaded.
2. Press **Plan it**. The model proposes a cleaner name and a folder for each file (up to 200 names per run).
3. Check the table of *From* and *To* paths.
4. Download **.sh** for macOS and Linux, or **.ps1** for Windows.
5. Put the script in the folder that contains the files and run it.

The plan follows a few rules: lowercase names with dashes, a date at the start when the name contains one, extensions never changed, and every input file used exactly once. Results that refer to names you did not provide are discarded, and characters that are illegal in file names are replaced.

Nothing is moved by the web page. The scripts do that, and the shell version uses `mv -n` so it will not overwrite an existing file. Read the table before you run a script.

### Setup

- **Ollama address** defaults to `http://localhost:11434`. Change it if Ollama runs on another port or another machine on your network.
- **Model** lists every model Ollama reports as installed.
- **Save and check** stores your choices and tests the connection.

On first load, if the saved model is not installed, Pigritia picks a Gemma model if it can find one, otherwise the first installed model.

---

## How it works

Pigritia is one self-contained HTML file with no dependencies. There is no server, no framework and no build step.

**Talking to Ollama.** The app uses two Ollama endpoints:

- `GET /api/tags` to list installed models and check the connection.
- `POST /api/chat` for all generation.

Free-text tools (Fridge list and Wind down) stream the response and display it as it arrives. Structured tools (Autopilot and File tidier) request `format: "json"` with a low temperature, then parse the JSON and render it. The fridge photo is sent as a base64 image in the message, which is how Ollama accepts images for vision models.

**Autopilot's contract.** The model is asked to return:

```json
{
  "summary": "two short sentences",
  "priority": "urgent | normal | low",
  "questions": ["things the reader must answer"],
  "tasks": [{ "task": "imperative action", "owner": "person or You", "due": "YYYY-MM-DD or empty" }],
  "reply": "ready-to-send reply in the chosen tone"
}
```

**File tidier's contract.**

```json
{ "files": [{ "from": "original name", "to": "clean name.ext", "folder": "Photos" }] }
```

**Browser features used.** `fetch` with streaming reads, the Web Audio API for ambience, `speechSynthesis` for reading aloud, canvas for resizing images, Blob downloads for scripts and calendar files, and `localStorage` for settings.

**Saved in your browser.** Three small values: the Ollama address, the selected model and the last page you viewed. Nothing else is stored.

---

## Privacy and data handling

- Requests go only to the Ollama address you set, which is `localhost` by default.
- Pigritia makes no analytics calls, loads no external scripts and uses no external fonts.
- Pasted text, photos and file names are held in the page while you use it and are not saved by Pigritia.
- If you point the address at another machine, your data goes to that machine. Only do that on a network you trust.

You can confirm all of this by opening your browser's network tab while using it.

---

## Choosing a model

Any model Ollama can serve will appear in Setup. Rough guidance:

| Need | Suggestion |
|---|---|
| Everything, including the fridge photos | A vision-capable Gemma such as `gemma3:4b` or larger |
| Text tools only, on modest hardware | A smaller Gemma variant |
| Better quality summaries, replies and plans | A larger Gemma or another strong instruction-tuned model |

Structured tools (Autopilot, File tidier) depend on the model producing valid JSON. Larger models are more reliable at this. If a run returns an error about unusable output, run it again or switch to a bigger model.

---

## Troubleshooting

**"Ollama offline" in the sidebar, or "Cannot reach Ollama".**
Ollama is not running, or the address is wrong, or the browser is blocking the page. Start Ollama with `OLLAMA_ORIGINS="*" ollama serve` (see Quick start), confirm the address in Setup, then press **Save and check**.

**Model "…" is not installed.**
Run `ollama pull <model>`, then reopen Setup and select it.

**Fridge list gives nonsense or an error.**
The selected model probably cannot read images. Pull a vision-capable model and select it in Setup.

**Autopilot or File tidier says the model returned no usable plan, or shows a parse error.**
The model did not return valid JSON. Try again, shorten the input, or use a larger model.

**It is slow.**
The first request after starting Ollama loads the model into memory, which takes time. Later requests are quicker. A smaller model also helps.

**No sound, or no voice, in Wind down.**
Browsers sometimes need a click before audio starts, so press the button again. The available voices depend on your operating system.

**The calendar button says nothing has a due date.**
Autopilot only creates events for tasks that have a date. Include dates in the text, or mention them in the paste.

---

## Limitations

- Output quality depends on the model you run. Always read a drafted reply before sending it.
- The File tidier works from names only. It cannot see what is inside a file, so odd names may be sorted into the wrong folder.
- The fridge reader can miss or misname items, especially in dim or cluttered photos.
- Calendar export creates all-day events and does not set times or reminders.
- Very long input is truncated to the most recent portion.
- The app is a front end only. It does not run Ollama for you.

---

## Customising

Because the whole app is one file, changes are straightforward.

- **Change what a tool does.** Each tool builds its prompt in one place in the script. Edit the system prompt text to change tone, format or rules.
- **Change the look.** Colours are CSS variables at the top of the stylesheet (`--canopy`, `--leaf`, `--moss`, `--fur`, `--mist`, `--hib`). The font stack is set on `body`.
- **Add a tool.** Add a sidebar link with a `data-go` value, a matching `<section id="…">`, and a handler that calls the shared `chat()` helper.
- **Change defaults.** The default address and model are set where the app first reads its settings.
- **Use another model family.** Nothing here is Gemma-specific. Select any installed model in Setup.

---

## Project structure

```
pigritia/
├── pigritia.html   # the entire app: markup, styles and script
└── README.md
```
