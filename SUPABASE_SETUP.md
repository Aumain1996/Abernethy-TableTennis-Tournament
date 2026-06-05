# Supabase Setup Guide — Match Data Persistence

This app uses [Supabase](https://supabase.com) (free) to store match scores persistently,
shared across all users visiting the Streamlit app.

---

## Step 1 — Create a Free Supabase Project

1. Go to [https://supabase.com](https://supabase.com) and sign in with GitHub or email
2. Click **New Project**, give it a name (e.g. `table-tennis`), set a database password, pick a region close to Perth
3. Wait ~1 minute for the project to provision

---

## Step 2 — Create the `matches` Table

In your Supabase project, go to **SQL Editor** and run this:

```sql
CREATE TABLE matches (
  match_key TEXT PRIMARY KEY,
  data      JSONB        NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Allow public read/write (fine for an internal tournament app)
ALTER TABLE matches ENABLE ROW LEVEL SECURITY;
CREATE POLICY "public_all" ON matches
  FOR ALL USING (true) WITH CHECK (true);
```

Click **Run**. You should see `Success. No rows returned.`

---

## Step 3 — Get Your API Keys

In your Supabase project go to:
**Project Settings → API**

Copy:
- **Project URL** — looks like `https://abcdefghij.supabase.co`
- **anon / public key** — a long JWT string

---

## Step 4 — Add Secrets to Streamlit Community Cloud

1. Go to your app on [share.streamlit.io](https://share.streamlit.io)
2. Click **⋮ (three dots) → Settings → Secrets**
3. Paste the following (replacing with your actual values):

```toml
[supabase]
url = "https://YOUR_PROJECT_REF.supabase.co"
key = "YOUR_ANON_PUBLIC_KEY"
```

4. Click **Save** — the app will automatically restart

---

## Step 5 — For Local Development

Create the file `.streamlit/secrets.toml` in this folder with the same content:

```toml
[supabase]
url = "https://your-project-ref.supabase.co"
key = "your-anon-public-key"
```

> ⚠️ Never commit this file to GitHub. It is already in `.gitignore`.

---

## How It Works

- Every time a match score is saved, it is **upserted** (inserted or updated) into the `matches` table in Supabase
- Every time the app loads (or any button is clicked), it **fetches all match data fresh** from Supabase
- This means all users see the same live data instantly
- If Supabase is not configured, the app falls back to a local `matches_data.json` file
