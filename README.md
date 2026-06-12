# AVGVST AI — National AI Compliance Registry (demo)

A single-page, dependency-free demo of a national EU AI Act (Regulation 2024/1689)
compliance registry, styled as an Estonian eAIDAS reference implementation. It is a
static mock-up: all data is hard-coded in the page, the "assessment engine" runs
entirely in the browser, and nothing is uploaded or persisted.

## What it does

- **Track A — Policy intake**: DPO/legal questionnaire (system identity, intended
  purpose, Annex III domain, decision type, documentation status, Art. 50 transparency).
- **Track B — Technical evidence**: model character, Art. 12 logging coverage matrix,
  log integrity / tamper-evidence, provider obligations and attestation, and an
  **evidence architecture (AEP)** section: event timing source (RFC 3161), independent
  verification path, portable evidence export, approval-event chaining, key history /
  CBOM, and signed retention policy — following the eIDAS trust-services mapping in
  Sokolov (2026), *Operationalizing the EU AI Act through eIDAS Trust Services
  Primitives*. Findings keep that paper's claim boundaries: artifacts *support evidence
  for* an article, never *satisfy* it; oversight is made *inspectable*, not *effective*.
  The findings panel shows a six-layer evidence status strip (provenance, logging,
  verifiability, oversight, trust architecture, crypto agility).
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

The **EST | ENG** switch in the header is functional: it toggles the interface
(labels, buttons, dropdown options, tabs, panel headings, statuses) between
Estonian and English and remembers the choice in the browser. Generated
analytical content — finding descriptions and statute quotations — stays in
English.

## Pitch deck

`pitch.html` is a self-contained interactive investor deck for AVGVST AI — 13 slides
following the standard structure (problem, solution, why now, market, business model,
moat, traction, financials, team, named risk, ask). Navigate with ← / →, swipe, or the
diamond dots; it includes a live countdown to the 2 August 2026 high-risk deadline, a
cost-of-waiting slider, a finding-routing demo of the hallucination partition, and
animated market/financial charts. Open it the same way as `index.html` (it is served
at `/pitch.html` on GitHub Pages). Figures are provenance-tagged in-slide as
Measured / Statute / Estimate / Placeholder — replace the placeholder items (team,
pilots, contact) before presenting.

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
