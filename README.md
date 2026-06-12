# AVGVST AI — National AI Compliance Registry (demo)

A single-page, dependency-free demo of a national EU AI Act (Regulation 2024/1689)
compliance registry, styled as an Estonian eAIDAS reference implementation. It is a
static mock-up: all data is hard-coded in the page, the "assessment engine" runs
entirely in the browser, and nothing is uploaded or persisted.

## What it does

- **Track A — Policy intake**: DPO/legal questionnaire (system identity, intended
  purpose, Annex III domain, decision type, documentation status, Art. 50 transparency).
- **Track B — Technical evidence**: model character, Art. 12 logging coverage matrix,
  log integrity / tamper-evidence, provider obligations and attestation.
- **Track C — Documents**: simulated AI-assisted field extraction with human confirmation.
- **Assessment panel**: findings graded Confirmed / Probable / Conditional / Flagged,
  fine-tier exposure (Art. 99(3)–(5), scaled against the turnover input using the
  "whichever is higher" rule), a source-verification log with article drawers, an
  expert review queue, and a gated sign-off checklist.
- **AI Memo (optional, powered by Mistral)**: sends the current form data and
  rule-engine findings to Mistral's API and renders a plain-language memo
  (bottom line, biggest risks, what to do next). Requires your own API key.

Five example systems (Bürokratt, Automatic Administrative Acts, AIRE, Top-10 Digital
Services, RIHAKE) are pre-loaded in the sidebar.

The visual theme follows Estonia's government design language: **Roboto** type and
the colour tokens (sapphire blue `#005aa3`, black-coral greys, jasper red, sea
green) come from the state's open-source
[CVI / Veera design system](https://github.com/e-gov/cvi). Only the AVGVST
wordmark keeps its Cinzel spaced-caps lettering.

> **Disclaimer:** This is a UI demo. The findings, fine figures, citations, and hashes
> are illustrative only and do not constitute legal advice.

## Run it locally

No build step and no dependencies — it is one HTML file.

```bash
# Option 1: just open it
open index.html            # macOS
xdg-open index.html        # Linux

# Option 2: serve it (recommended so the Google Fonts load consistently)
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Run it on GitHub (GitHub Pages)

This repo ships with a Pages deployment workflow
(`.github/workflows/deploy-pages.yml`). To enable it:

1. Go to **Settings → Pages** in this repository.
2. Under **Build and deployment → Source**, select **GitHub Actions**.
3. Push to `main` (or run the **Deploy to GitHub Pages** workflow manually from the
   **Actions** tab via *Run workflow*).

The site will be published at `https://<owner>.github.io/<repo>/`.

## Optional: live AI analysis with Mistral

The rule engine is deterministic JavaScript; the **AI Memo** tab adds a real LLM on
top of it, using Mistral.

1. Create a (free-tier) account at https://console.mistral.ai and generate an
   **API key** under *API Keys*.
2. Open the app, paste the key into **"Mistral API key"** in the sidebar (bottom
   left) and pick a model. The key is stored only in your browser's localStorage —
   it is never written into the site or the repository.
3. Load or fill in a system, run **Generate compliance assessment**, then open the
   **AI Memo** tab and click **Generate AI memo with Mistral**.

The browser calls `https://api.mistral.ai/v1/chat/completions` directly with your
key. That is fine for personal use of a demo, but do not hardcode a key into the
page or share a key-loaded browser: anyone with the key can spend your quota. A
production setup would route the call through a small server-side proxy instead.

## Repository layout

```
index.html                          # the entire app (HTML + CSS + JS)
.github/workflows/deploy-pages.yml  # GitHub Pages deployment
```
