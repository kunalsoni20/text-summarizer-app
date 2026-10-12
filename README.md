---
title: Text Summarizer App
emoji: 📝
colorFrom: orange
colorTo: yellow
sdk: docker
app_port: 7860
---

# Text Summarizer App

A FastAPI web application that summarizes text using a locally saved T5
Transformer model. The project includes a simple browser interface and a JSON
API endpoint.

## Features

- Text summarization through a browser-based UI
- FastAPI backend with a `/summarize/` endpoint
- Local model inference using PyTorch and Hugging Face Transformers
- Automatic CPU, CUDA, or Apple MPS device selection

## Requirements

- Python 3.10 or newer
- Git LFS for the saved model weights
- Windows, macOS, or Linux

## Setup

Clone the repository:

```bash
git clone https://github.com/kunalsoni20/text-summarizer-app.git
cd text-summarizer-app
```

Create and activate a virtual environment:

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

macOS/Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

The model weights are tracked with Git LFS. If Git LFS is installed, they are
downloaded by the clone. If the model directory contains an LFS pointer instead
of the actual weights, run:

```bash
git lfs install
git lfs pull
```

The application expects the model files in:

```text
saved_summary_model/
```

## Run the application

```bash
python -m uvicorn app:app --reload
```

Open <http://127.0.0.1:8000> in a browser.

The first startup loads the model and may take a few seconds. CPU inference
can also take several seconds for each summary.

## Deploy on Hugging Face Spaces

Create a new Space with **Docker** as the SDK, then upload or push this
repository to the Space. The included `Dockerfile` starts FastAPI on port
`7860`, which is the port configured in the Space metadata above.

The Space may take a few minutes to build and start because it installs
PyTorch and loads the local model weights.

## API usage

Send a POST request to `/summarize/`:

```bash
curl -X POST http://127.0.0.1:8000/summarize/ ^
  -H "Content-Type: application/json" ^
  -d "{\"dialogue\":\"The team discussed the project schedule and agreed to test the application tomorrow.\"}"
```

Example response:

```json
{
  "summary": "The team agreed to test the application tomorrow."
}
```

## Project structure

```text
.
├── app.py
├── index.html
├── requirements.txt
└── saved_summary_model/
```

## Notes

The model is stored with Git LFS because its weight file is larger than
GitHub's regular file-size limit. GitHub LFS storage and bandwidth quotas may
apply to the repository.
