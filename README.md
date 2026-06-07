# Truth Is Not Enough — 10 Senses Model (10SM) & Training Confidence Index (TCI)

Companion repository for the paper *"Truth Is Not Enough: An Epistemological Framework for
Multi-Dimensional AI Output Evaluation with Empirical Validation in Human-AI Coaching"*
(EJC 2026 — 36th International Conference on Information Modelling and Knowledge Bases,
Vaasa, Finland, June 8–12, 2026).

The work argues that scientific accuracy is a **necessary but not sufficient** condition for a
reliable AI output, and proposes a unified *Truth–Good–Beauty* framework — the **10 Senses Model
(10SM)** — operationalized as the **Training Confidence Index (TCI)**. The framework is validated
on five months of operational data from the EKIDEN.AI human–AI running-coaching platform.

🔗 **Project site:** the interactive introduction (`index.html`) — open it locally or host it
statically.
📄 **Paper:** EJC 2026 / IOS Press (forthcoming).

---

## What's in this repository

| Path | Description |
| --- | --- |
| `index.html` | The project website — framework explainer **and** an interactive, data-driven demo (M1 alert explorer, discrepancy figures, dataset infographic). Self-contained: no build step, no backend, no keys. |
| `data/aggregates.json` | The **aggregate, non-identifying** dataset behind every figure on the site. |
| `README.md` | This file. |

> The website embeds the same numbers found in `data/aggregates.json` so it renders offline;
> `aggregates.json` is the citable source of those values.

---

## Data & privacy

**Only aggregate, non-identifying data is published here.** This repository contains **no**
individual-level records, **no** user identifiers, **no** free-text fields, and **no** location or
GPS data.

The published figures are either (a) aggregate statistics computed over the full cohort, or
(b) de-identified summary figures already reported in the paper. Under the GDPR this corresponds to
**anonymous information** (Recital 26) — it does not relate to an identified or identifiable person —
and is therefore outside the scope of personal-data processing.

What was deliberately **excluded** to prevent re-identification:

- direct/pseudonymous identifiers (platform user IDs, coach IDs);
- free text from coaching dialogues (which may contain places or personal details);
- per-activity GPS / FIT-file references, timestamps, and other quasi-identifiers;
- record-level rows of any kind.

Small categories are bucketed (e.g. languages outside the top four are aggregated as *Other*) so that
no aggregate cell can single out an individual.

**Record-level data** (used for the full statistical analysis in the paper) is **not** released
openly. It remains personal data — and includes health-related fields (VDOT, VO₂max, heart rate,
HRV, sleep) that are *special-category* data under GDPR Art. 9. It can be made **available on request
under a controlled-access data-use agreement** for legitimate scientific research, subject to the
original consent scope and applicable safeguards (GDPR Art. 89 / Japan APPI).

---

## `data/aggregates.json` — data dictionary

| Key | Meaning |
| --- | --- |
| `cohort` | Headline totals: runners, activities, coaching dialogues, total distance (km), total moving hours. |
| `fitness_level_vdot` | VDOT (Daniels) summary — count, min/mean/max, and number of elite runners (VDOT ≥ 70). |
| `activity_types` | Count of activities by type (Run, Ride, Walk, …). |
| `language_mix` | Runner count by interface language; minor languages bucketed as `other`. |
| `rpe_distribution` / `rpe_total` | Distribution of logged Rate of Perceived Exertion (1–10). |
| `mood_distribution` / `mood_total` | Distribution of logged mood values. |
| `subjective_objective_discrepancy` | The paper's Table 9 finding: of 890 sessions with both RPE and objective intensity, 17.1% (152) showed a subjective–objective mismatch (87 "felt harder", 65 "felt easier"). |

All counts are derived from a snapshot dated `2026-02-13`.

---

## Reproducing the figures

The site reads its numbers from `aggregates.json` (mirrored inline). To regenerate the visuals,
edit the values in `aggregates.json` and the corresponding inline constants in `index.html`
(`RPE_DIST`, the `hbars(...)` calls, and the discrepancy donut), or wire the page to `fetch()` the
JSON when served over HTTP.

## Repository

```
https://github.com/aix-sc/10senses-model
```

Suggested layout:

```
10senses-model/
├── index.html            # the site (self-contained)
├── data/
│   └── aggregates.json    # aggregate, non-identifying dataset
├── firebase.json          # hosting config
├── LICENSE                # MIT (code)
├── DATA_LICENSE           # CC BY 4.0 (data)
└── README.md
```

## Committing changes (Git)

First-time setup:

```bash
git init
git remote add origin https://github.com/aix-sc/10senses-model.git
git add .
git commit -m "Add 10SM/TCI site and aggregate dataset"
git branch -M main
git push -u origin main
```

Day-to-day:

```bash
git add index.html data/aggregates.json   # or: git add -A
git commit -m "Update infographic and JA copy"
git push
```

Before committing, double-check no secrets slipped in: `git diff --staged | grep -iE "key|token|secret|password"` should return nothing.

## Running locally / deploying (Firebase Hosting)

It's a single static file — no build step.

```bash
# quickest local preview
python3 -m http.server 8000        # open http://localhost:8000
```

Deploy to Firebase Hosting (the platform EKIDEN.AI / AIx already use):

```bash
npm install -g firebase-tools       # one-time
firebase login                      # one-time
firebase init hosting               # one-time: pick the project, set public dir to "."
firebase deploy                     # ship it
```

Minimal `firebase.json` if you'd rather not run `init`:

```json
{
  "hosting": {
    "public": ".",
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**", "README.md"]
  }
}
```

Optional CI (matches the AIx GitHub Actions convention): add a workflow that runs
`firebase deploy --only hosting` on push to `main` using a `FIREBASE_TOKEN` repo secret.

No API keys, environment variables, or service-account credentials are required or present in the
site — please keep it that way before pushing.

---

## Citation

```bibtex
@inproceedings{takahashi2026truth,
  title     = {Truth Is Not Enough: An Epistemological Framework for Multi-Dimensional
               AI Output Evaluation with Empirical Validation in Human-AI Coaching},
  author    = {Takahashi, Yusuke},
  booktitle = {Information Modelling and Knowledge Bases XXXVII (EJC 2026)},
  publisher = {IOS Press},
  year      = {2026},
  address   = {Vaasa, Finland}
}
```

## License

This repository carries two licenses — one for code, one for data — which is standard practice and
keeps each kind of reuse unambiguous:

- **Code** (`index.html`, config): **MIT** — see `LICENSE`. Permissive, widely understood, lets
  anyone reuse the demo/visualizations with attribution and no copyleft obligations.
- **Aggregate data** (`data/aggregates.json`): **CC BY 4.0** — see `DATA_LICENSE`. The standard
  license for open research data; reusers must credit the source. (If you want to bar commercial
  reuse, CC BY-NC 4.0 is the alternative — but it complicates academic reuse, so CC BY 4.0 is
  recommended for a conference companion repo.)

These cover only the contents of this repository (aggregate data and the site). They do **not** grant
any rights to the underlying record-level EKIDEN.AI data, which is not published here.

## Contact

Yusuke Takahashi — Asia AI Institute · AIx{} · Tokyo, Japan
Platform: [EKIDEN.AI](https://ekiden.ai) · Conference: [EJC 2026](https://sites.uwasa.fi/ejc2026/)
