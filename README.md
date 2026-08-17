# AI Governance Control Room

A single-page, interactive portfolio demo showing how an AI/ADAS safety governance review actually works — from a flagged incident, through AI performance data and safety framing, to a traced evidence trail, to a real governance decision.

**File:** `index.html` — one self-contained file, no build step, no backend, no login. Opens directly in any browser.

---

## What it's for

Built to show a recruiter or hiring manager how the author thinks about AI safety governance for autonomous/ADAS systems — not just "the model has a bug," but the full chain of reasoning a safety reviewer goes through: which standard applies, what evidence exists, who owns it, what's missing, and what the actual call should be.

Every number and scenario on the page is fictional and clearly tagged as synthetic — there's a persistent banner and per-panel tags, by design, so nothing on the page could be mistaken for a real incident, model, or company.

## The scenario

A fictional vehicle (`VH-042`, Level 2 ADAS) approaches a marked crosswalk at night in light rain. Its camera-based pedestrian detector is 92% confident it sees a person — then that confidence drops to 31% within ~400ms as the lighting/weather conditions worsen. The car's automatic emergency braking (AEB) only fires above 50% confidence, so it doesn't brake. The driver brakes manually; no collision. Logged as a near-miss safety event for review.

## Structure

**1. Opening animation (~8 sec)**
A 5-frame CSS/SVG sequence dramatizes the incident (vehicle active → pedestrian detected → confidence dropping → AEB threshold missed → "SAFETY EVENT DETECTED" freeze frame), skippable, replayable.

**2. Incident summary**
Persistent card with the vehicle ID, timestamp, location, conditions, and a plain-language description of what happened.

**3. AI Performance tab**
A table of the model's precision/recall across four conditions (daylight/clear, night/clear, daylight/rain, night/rain). The night+rain row is auto-flagged red — it's the model's weakest tested condition, and it's exactly the condition this incident happened in.

**4. Safety (SOTIF) tab**
Frames the hazard correctly: this is a *performance limitation* (a working system just not reliable enough in a hard condition) under **ISO 21448 (SOTIF)**, not a broken-part malfunction under **ISO 26262**. Includes a prominent disclaimer that this tool oversimplifies for demo purposes, and an interactive Severity / Exposure / Controllability rating exercise.

**5. Evidence & Traceability tab**
A clickable chain: **Hazard → Safety Goal → Requirement → Model → Test → Evidence → Decision**. Two nodes (the requirement `REQ-AEB-017` and the incident evidence bundle `EVD-0417`) expand into full evidence records — each item shows its type, which team owns it, where it came from, who it goes to next, its status (received/missing), and a concrete next step. A traceability percentage is calculated live from how many items are actually present, so it can never drift out of sync with the checklist. Every node also shows which team owns that stage, and a chip trail visualizes the full pipeline: Requirements Team → ML Development Team → Test & Validation Team → Independent Safety Team → Governance Board.

**6. Governance decision**
Three choices — Approve / Approve with Conditions / Block — each returning a distinct, reasoned response (not just a rubber stamp) that weighs the AI weakness and evidence gaps against real tradeoffs like cost and safety.

## Key design decisions

- **Synthetic data is never buried in a footnote.** A sticky top banner plus per-panel tags make it unmissable.
- **SOTIF vs. ISO 26262 framing is explicit**, correcting an earlier audit finding that only cited 26262.
- **Numbers are computed, not hardcoded**, where it matters (traceability %, evidence counts) — reduces the risk of numbers silently drifting out of sync if the underlying list changes.
- **Plain language throughout** — technical terms a real reviewer would expect (ISO numbers, precision/recall, ODD, HARA) are kept, but bureaucratic phrasing was deliberately stripped out in favor of direct sentences.

## Tech stack

Plain HTML + vanilla JavaScript + Tailwind CSS (via CDN). No React, no bundler, no dependencies to install. All "data" (AI performance table, evidence records, decision reasoning) lives in JS objects near the top of the `<script>` tag, so it's straightforward to extend or restyle.

## Not yet done

- Deployment to a shareable link (Vercel/Netlify/GitHub Pages)
- A short screen-recording walkthrough video
- A sanity-check of the SOTIF/safety-framing language by someone with real functional-safety background — flagged from the start as the part most likely to get scrutinized
