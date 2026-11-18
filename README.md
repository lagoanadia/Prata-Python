# 🇸🇪 Prata — Python Language App

A desktop app for learning Swedish through AI conversation and vocabulary tracking — built with Python, a local **Ollama** model and **CustomTkinter**. Part of the *Prata* family of language-learning projects.

## 📁 Project Structure

```
├── app.py               → Main UI: chat, vocabulary list and quiz mode
├── ai.py                → Ollama chat with conversation history and the tutor's system prompt
├── vocab.py             → Vocabulary CSV CRUD (save, list, translate, delete)
├── quiz.py              → Quiz CSV CRUD and word status tracking
└── data/
    ├── vocabulary.csv   → Saved words and translations
    └── quiz.csv         → Word status (unlearnt / learnt)
```

## 🚀 How It Works

The window is split in two: the **AI chat** on the left and the **vocabulary list** on the right. A third screen holds the **quiz**.

- **AI Chat** — a beginner (A1) Swedish tutor running on a local `llama3.2` model. Every Swedish sentence comes with its English translation in parentheses, and mistakes are gently corrected.
- **Vocabulary Tracker** — add words you meet in the conversation with their translation; they're saved to CSV and loaded on startup.
- **Quiz Mode** — flashcards built from your *unlearnt* words; marking one as learnt updates its status in real time.

### The model has no memory — so we send it all

Ollama is stateless, so `ai.py` keeps a list of `{role, content}` dictionaries and passes the full history on every call:

```python
chat_history.append({"role": "user", "content": user_message})
```

### A strict system prompt

The tutor's behaviour lives in `SYSTEM_PROMPT`: A1 level only, max 3 sentences, everyday topics, and *always* an English translation after every Swedish sentence.

### CSV as a tiny database

`vocab.py` and `quiz.py` create their files if missing, append rows with `csv.writer`, and update a word's status by reading all rows with `csv.DictReader`, modifying them and rewriting the file. Deleting a word removes it from both files.

### Responsive UI

The AI call runs in a separate `threading.Thread` so the window doesn't freeze while the model answers.

## 🧠 Key Concepts

- **Local LLMs with Ollama** — chat API, system prompt, conversation history
- **Prompt engineering** — constraining level, format and length
- **CustomTkinter** — frames, textboxes, scrollable frames
- **Threading** — keeping a GUI responsive during slow calls
- **CSV persistence** — create / read / update / delete with the `csv` module
- **Modular code** — UI, AI and data access in separate files

## 🛠️ How To Run

```bash
pip install customtkinter ollama
```

Install [Ollama](https://ollama.com), then download the model:

```bash
ollama pull llama3.2
```

```bash
python app.py
```

---

*Built as a personal language learning tool during my first year of DAM (Desarrollo de Aplicaciones Multiplataforma).*
