# NeuroTune – Chatbot Fine‑Tuning Agent

## 📦 Project context (MonoOS)
- **Language:** Python (3.10+)
- **Target use‑case:** Train a conversational model on custom dialogue data that can later be called from a userspace daemon running on MonoOS.
- **Current status:** The repository contains a ready‑to‑run script (`neurotune_finetune.py`) that uses 🤗 Transformers, `torch`, `datasets` and `accelerate`. All dependencies are installable via `pip` inside a virtual environment.

## 🤖 Agent responsibility
| Responsibility | Description |
|----------------|-------------|
| **Set up a Python virtual environment** and install the required libraries (`torch`, `transformers`, `datasets`, `accelerate`). | The agent writes the exact commands (`python -m venv .venv && source .venv/bin/activate && pip install …`). |
| **Validate training data** (`data/train.jsonl`, optional `data/valid.jsonl`). | Checks that each line is valid JSON and contains the keys `instruction` and `response`; reports malformed lines. |
| **Run the fine‑tuning script** with user‑specified hyper‑parameters (base model, epochs, batch size, learning rate, output directory). | Calls `python neurotune_finetune.py …` and streams the trainer output. |
| **Explain errors** (e.g., missing `torch`, CUDA OOM, incorrect model name). | Provides clear troubleshooting steps and suggested fixes. |
| **Provide a quick test snippet** to load the newly saved model and generate a reply. | Generates a short Python snippet using `pipeline`. |
| **Optional push‑to‑Hugging Face** workflow (Git‑LFS, `huggingface‑hub`). | Guides the user through `huggingface-cli login`, repository creation, and `git push`. |

## 🎯 Prompt template (what you should send NeuroTune)
```
You are the NeuroTune agent.
The repository root contains `neurotune/neurotune_finetune.py` and a `data/` folder with training files.
Your task is to **[ACTION]**.

Possible actions:
- set up a virtual environment and install the required Python packages
- validate `data/train.jsonl` (and optionally `data/valid.jsonl`) and list any malformed lines
- fine‑tune a specific model (e.g., `meta-llama/Meta-Llama-3-8B-Instruct`) with given hyper‑parameters and store the result under `fine_tuned/`
- explain why the script fails with a particular error message (provide the full traceback)
- generate a short Python snippet that loads the fine‑tuned model and produces a reply
- give step‑by‑step instructions for uploading the model to Hugging Face

All file paths are relative to the repository root. Do not ask for additional information unless the request is ambiguous.
```

### Example prompts
- *“Fine‑tune `meta-llama/Meta-Llama-3-8B-Instruct` on `data/train.jsonl` for 3 epochs, batch‑size 4, learning‑rate 5e‑5, and write the model to `fine_tuned/`.”*
- *“Validate `data/train.jsonl` and tell me which lines are not valid JSON or missing `instruction`/`response`.”*
- *“Explain why I get `ImportError: No module named 'torch'` even after activating my virtualenv.”*
