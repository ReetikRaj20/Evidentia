# Evidentia — Development Log (project-wide)

A running record of what I built, what broke, how I fixed it, and what I learned.
Times are local (Europe/Berlin). Newest entries at the top.

This file covers **project-wide** work: setup, tools, Git, API keys, and decisions that affect the whole project.
Each notebook has its **own devlog** next to it in `notebooks/`.

## Notebook devlogs

| Notebook | Devlog | Purpose |
|---|---|---|
| `01_openalex_search.ipynb` | [01_openalex_search.devlog.md](notebooks/01_openalex_search.devlog.md) | Send a claim to OpenAlex, understand the response, test how far search ranking gets towards evidence |

---

## 2026-09-27

### 13:42 — Pushed the first OpenAlex work to GitHub

Commits: "use openalex to search for papers to verify your claim" (13:42) and "push content of 01_openalex_search.ipynb onto Github" (13:44).

### 13:32 — Created an OpenAlex account and stored the API key as an environment variable

**What I did**

Created a free account on OpenAlex, copied my API key from openalex.org/settings/api, and saved it as a Windows user environment variable (in a new PowerShell window, then restarted VS Code):

```
setx OPENALEX_API_KEY "<my key>"
```

**Why**
- An environment variable lives in my Windows user settings, not in any file in the project folder, so it can never be committed or pushed to GitHub, and tools that can only see the project folder can't read it.
- A free key gives 10x the keyless OpenAlex allowance ($1 of usage per day, about 10,000 normal searches).
- The key is sent as a header (`Authorization: Bearer ...`) instead of a URL parameter, so it doesn't appear in URLs or error messages.
- I never print the key: Jupyter saves cell outputs inside the `.ipynb` file, so a printed key would be pushed with the notebook.

### 13:32 — Switched from Semantic Scholar to OpenAlex

**Why:** Without a key, Semantic Scholar shares one rate limit across all keyless users, so requests often fail with `429 Too Many Requests`, and getting a personal key requires an application that isn't approved instantly. OpenAlex works straight away and a free key takes about 30 seconds to create. (I may still request a Semantic Scholar key later and compare the two.)

### 12:47 — Created `requirements.txt`

Lists the packages the project depends on, so anyone can rebuild the environment with `pip install -r requirements.txt`.

### 12:43 — Created `.gitignore`

Excluded `.venv/`, `__pycache__/`, `.ipynb_checkpoints/` and `.env` from Git, so the environment, auto-generated files and (later) API keys never get uploaded to GitHub.

### 12:11 — Set up a Python virtual environment

**What I did**

```
python -m venv .venv
.venv\Scripts\activate
pip install requests jupyter
```

**Why**
- `python -m venv .venv` creates a private Python environment for this project, so its packages don't mix with other projects on my computer.
- `.venv\Scripts\activate` switches the terminal into that environment.
- `requests` is for calling web APIs (e.g. OpenAlex); `jupyter` is for running notebooks.
