# Mythos-GPT

A streaming, local-first CLI chatbot. Talks to any OpenAI-compatible endpoint —
by default your own local llama.cpp server. Ships with an editable ASCII banner,
an editable persona (system-prompt.txt), and live output instead of a dead screen.

## Features

- Streaming replies — text appears as it generates (no more waiting in silence)
- Local-first — works with llama.cpp on 127.0.0.1:8080, or any OpenAI-compatible API
- Reasoning models supported — thinking is disabled on local llama.cpp; cloud models keep their own reasoning
- UTF-8 safe streaming — emojis survive the pipe
- Editable banner — put your own ASCII art in `banner.txt`
- Editable persona — `system-prompt.txt` is the system prompt, read fresh every message
- Name correction — a stale trained identity in the model gets rewritten at display
- Persistent config — `mythosgpt_config.json` (copy from `config.example.json`)

## Requirements

- Python 3.8+
- pip

## Install

    git clone <your-repo-url> mythosgpt
    cd mythosgpt
    pip install -r requirements.txt
    cp config.example.json mythosgpt_config.json

## Run

    python ai.py

Menu: 4 = Start Chat, 1 = language, 2 = model, 3 = API key, 5 = exit.

## Pointing it at your own model (recommended)

Run a llama.cpp server with your GGUF:

    llama-server -m /path/to/model.gguf --port 8080 --host 0.0.0.0 -ngl 0 --ctx-size 2048 --threads 6

Then set `base_url` to `http://127.0.0.1:8080/v1` in `mythosgpt_config.json` (the default).
The API key is ignored by llama.cpp — any value works. First reply can take a minute
on CPU — the thinking line tells you it's alive; wait for it.

## OpenRouter mode

Set `base_url` to `https://openrouter.ai/api/v1`, add a real key, and pick a model
from the menu (option 2). Free `:free` models work.

## Any cloud model (OpenAI-compatible)

Works with ANY OpenAI-compatible API — OpenRouter, Groq, NVIDIA NIM, Together,
vLLM, Gemini's OpenAI-compat endpoint, etc. No file edits needed: environment
variables override the config file, so one install talks to everything:

    MYTHOSGPT_BASE_URL=https://openrouter.ai/api/v1
    MYTHOSGPT_API_KEY=sk-or-...
    MYTHOSGPT_MODEL=some/model-id

    MYTHOSGPT_BASE_URL=... MYTHOSGPT_API_KEY=... MYTHOSGPT_MODEL=... python ai.py

`MYTHOSGPT_LANGUAGE` (e.g. `Portuguese`) works too. Anything not set falls back
to `mythosgpt_config.json`, then to the local llama.cpp defaults. The llama.cpp
`chat_template_kwargs` knob is only sent to local servers — cloud APIs that
reject unknown fields stay happy.

## Files

| File | Purpose |
|---|---|
| `ai.py` | the whole client (one file) |
| `banner.txt` | ASCII header art — edit freely, read on every launch |
| `system-prompt.txt` | the persona — edit freely, read on every message |
| `config.example.json` | config template (copy to `mythosgpt_config.json`) |
| `requirements.txt` | dependencies |

## Notes

- `mythosgpt_config.json` is git-ignored so real API keys never get committed.
- Set the repo URL in `ai.py` (`SITE_URL`) to wherever you host this.
