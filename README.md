# Dual GPT Chat Orchestrator

A small example project that orchestrates two chat models (an argumentative assistant and a conciliatory assistant) using the OpenAI Python client. This repository demonstrates how to seed two separate chat agents with different system prompts and have them take turns conversing. It is implemented as a simple script intended for use in a Jupyter notebook environment.

---

## Project overview

* **Purpose:** Show how to run two different chat models with distinct system personalities and exchange messages between them programmatically.
* **Key behaviors:**

  * `gpt1` uses a *snarky, argumentative* system prompt.
  * `gpt2` uses a *polite, agreeable* system prompt.
  * Both models are driven by message lists that get appended as the conversation develops.
* **Intended environment:** Jupyter notebooks (IPython display helpers are used), but the code can be adapted to plain Python scripts.

---

## Features

* Clear separation of system prompts and user/assistant message histories.
* Example loop that runs multiple turns and displays the exchange in a human-friendly format.
* Configurable models and prompts via environment variables.

---

## Prerequisites

* Python 3.9 or newer.
* `pip` to install dependencies.
* An OpenAI API key stored in an environment variable.

---

## Installation

1. Create and activate a Python virtual environment (optional but recommended).

2. Install the required Python packages:

```bash
pip install python-dotenv openai ipython
```

> Note: The example uses `IPython.display.Markdown` for nicer output inside notebooks. If you run as a script, you can replace these with simple `print()` calls.

---

## Environment variables

Create a `.env` file in the project root (or export the variables in your shell). At minimum provide:

```
OPENAI_API_KEY=your_api_key_here
```

The script loads env vars with `python-dotenv`.

---

## Configuration

Open the script and edit the following variables to change behaviour:

* `gpt1_model` — model identifier used for the argumentative assistant (default in example: `gpt-4o-mini`).
* `gpt2_model` — model identifier used for the polite assistant (default in example: `gpt-4.1-mini`).
* `gpt1_system` — the system prompt for the first assistant (argumentative persona).
* `gpt2_system` — the system prompt for the second assistant (polite persona).
* `gpt1_messages`, `gpt2_messages` — initial seed messages for each assistant.

The README intentionally uses placeholder model names. Replace them with models available to your account if necessary.

---

## How it works (high level)

1. The script creates message lists that include a system message for each agent.
2. In each loop iteration, it calls `call_gpt1()` to get a reply from model 1 and appends that reply to `gpt1_messages`.
3. It then calls `call_gpt2()` to get a reply from model 2 and appends that reply to `gpt2_messages`.
4. Both responses are displayed using `IPython.display.Markdown`.

This setup simulates two agents talking to each other while keeping separate histories and system personalities.

---

## Usage

Run the notebook or script from a Python environment that has the environment variable configured.

If running inside a notebook, the provided script will show nicely formatted Markdown blocks for each message. If running as a plain Python script, replace `display(Markdown(...))` with `print(...)`.

Example (run in a notebook cell):

```python
# run the script or copy the contents into a notebook cell
python your_script_name.py
```

---

## Example configuration snippet (from the repository)

```python
gpt1_model = "gpt-4o-mini"
gpt2_model = "gpt-4.1-mini"

gpt1_system = (
    "You are a chatbot who is very argumentative; "
    "you disagree with anything in the conversation and you challenge everything, in a snarky way "
)

gpt2_system = (
    "You are a very polite, courteous chatbot. You try to agree with "
    "everything the other person says, or find common ground. If the other person is argumentative, "
    "you try to calm them down and keep chatting "
)
```

---

## Troubleshooting

* **API key not found:** Make sure `OPENAI_API_KEY` is set and that `load_dotenv()` is loading your `.env` file. The script prints a short message if no key is found.
* **Model errors:** If the API returns model-not-found or permission errors, replace the model identifiers with ones available to your account.
* **Rate limits or timeouts:** Handle exceptions around API calls and implement retry/backoff logic if you plan to run long conversations.

---

## Extending the project

* Add logging to persist full conversation transcripts to disk.
* Persist message histories so the conversation can be resumed between runs.
* Add a simple web UI for live interaction between the two agents.
* Introduce moderation or safety checks if you plan to deploy publicly.

