AI Text Summarizer

A simple, modern browser UI (HTML/CSS/JS) for text summarization backed by a Flask API using Hugging Face Transformers (default model: facebook/bart-large-cnn).

✨ Features

Clean, responsive dark UI (Inter + Orbitron fonts)

Live character/word counters (input & output)

Adjustable summary length (min/max tokens)

Loading state, error/success feedback, copy-to-clipboard

Client-side validation + server-side validation

CORS-enabled API for easy front-end ↔ back-end calls

🗂️ Project Structure

AI_TextSummarizer/
├─ index.html               # Front-end UI (HTML + inline JS)
├─ AI_TextSummarizer.css    # Front-end styles
└─ server.py                # Flask API (POST /summarize)

Your index.html currently fetches the API at http://127.0.0.1:5000/summarize.

🧰 Tech Stack

Frontend: HTML, CSS, Vanilla JS

Backend: Python, Flask, Flask-CORS

NLP: Hugging Face transformers (default: facebook/bart-large-cnn)

Runtime: PyTorch (default for the provided pipeline)

✅ Prerequisites

Python 3.9 – 3.12 (recommended)

pip available on your PATH

Disk space for model downloads (BART-Large-CNN ≈ ~1.6–2 GB on first run)

If you prefer a smaller model: switch to t5-small (faster, less accurate) in server.py.

🔧 Setup & Local Run

1) Create a virtual environment

# Windows (PowerShell)
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# macOS/Linux
python3 -m venv .venv
source .venv/bin/activate

2) Install dependencies

pip install --upgrade pip
pip install flask flask-cors transformers torch sentencepiece

Notes:

torch install may vary depending on CPU/GPU. The generic CPU wheel above is usually fine. For GPUs, install the correct CUDA wheel from the official PyTorch instructions.

sentencepiece is useful for some models (e.g., T5, Pegasus). It won’t hurt to include it.

3) Start the Flask API

python server.py

This starts the API at http://127.0.0.1:5000.

4) Open the frontend

Simply double‑click index.html to open in your browser or

Serve the folder with a simple static server (optional):

# Using Python's http.server
python -m http.server 8080
# then open http://127.0.0.1:8080/index.html

The frontend will call the API at http://127.0.0.1:5000/summarize. Keep the API window running.

🔌 API Reference

POST /summarize

Request headers

Content-Type: application/json

Request body (JSON)

{
  "text": "<your long input text>",
  "max_length": 130,
  "min_length": 30
}

text (string, required): The text to summarize (server enforces minimum 50 characters).

min_length (int, optional): Minimum tokens in the summary. Default 30.

max_length (int, optional): Maximum tokens in the summary. Default 130.

Response (200)

{ "summary": "<generated summary>" }

Validation & limits (as coded)

Input text: 50 ≤ length ≤ 15,000 characters

min_length: 10–1000

max_length: 50–1000

min_length must be < max_length

Possible errors

400 — invalid JSON, short/empty text, invalid lengths

500 — model not loaded or unexpected runtime error

⚙️ Configuration Tips

Model choice (in server.py):

# summarizer = pipeline("summarization", model="t5-small")
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")
# summarizer = pipeline("summarization", model="google/pegasus-large")

CORS: currently enabled for all origins via CORS(app). For production, restrict to your domain:

CORS(app, resources={r"/summarize": {"origins": ["https://your-site.com"]}})

Port: API runs on :5000 (change via app.run(debug=True, port=5000)). If you change the port, update the fetch(...) URL in index.html.

