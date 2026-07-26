# Abernethy Road Table Tennis Tournament

A Streamlit app for running the 2026 Abernethy Road Table Tennis Tournament. It manages a 56-player, single-elimination competition from the initial draw through to the grand final.

## What the app does

- Displays the full knockout bracket, including first-round byes
- Uses a fixed random seed so the draw remains consistent between sessions
- Lets organisers enter a match date and scores for up to three sets
- Validates table tennis scoring: first to 11, with a two-point lead required at deuce
- Supports recording forfeits
- Automatically advances winners into later rounds
- Shows a round-by-round summary and highlights the tournament champion
- Builds a live ladder with match, set, and point statistics
- Includes the tournament format, match rules, and code of conduct

Match results are stored in Supabase when it is configured. If Supabase is unavailable, the app falls back to a local `matches_data.json` file.

## Run locally

1. Create and activate a Python virtual environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

2. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Start the app:

   ```bash
   streamlit run streamlit_app.py
   ```

Streamlit will print the local address to open in a browser.

## Optional Supabase setup

To share results across deployments and users, create a Supabase table named `matches` with:

- `match_key`: text, primary key
- `data`: JSON/JSONB

Then add `.streamlit/secrets.toml`:

```toml
[supabase]
url = "https://your-project.supabase.co"
key = "your-supabase-key"
```

Do not commit real credentials to the repository. Without these settings, results are saved locally instead.

## Project files

- `streamlit_app.py` — application UI, bracket logic, scoring, and persistence
- `requirements.txt` — Python dependencies
- `Abernethy Rd Table Tennis Comp Registrations.csv` — registration list
- `Table Tennis Trophy.jpg` — image displayed above the grand final

The current player list is embedded in `streamlit_app.py`; the registration CSV is retained as the source data but is not loaded at runtime.
