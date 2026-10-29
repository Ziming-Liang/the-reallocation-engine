---
status: DRAFT
todos_open: 3
last_gate: null
attestation: null
recipe_version: 0.1.0
---

# cognitive-fit-swe — AI-Substitution Fit Check for Software/AI Engineer Roles

## Executive summary

This document is a job-search checklist for one specific kind of person: an F-1
international student finishing a Software Engineering master's degree who is
applying to "Software Engineer" / "AI Engineer" roles — the exact job family
that AI tools are most often claimed to be coming for. Before spending limited
application effort on a posting, it answers three questions with real data
instead of a guess: *is this specific job title the kind of work that leans on
human judgment (safer from AI substitution) or on boilerplate implementation
(more easily commoditized)? Has this company actually sponsored H-1Bs for
people in this role family before? And is the posting even still live?* It
also names, up front, a real gap found while building this: the repository's
own scoring script currently computes the "judgment-resilience" signal and
then silently throws it away — a 0% weight, not a bug announcement. Read the
rest of this file for exact commands, what it can and cannot check, and what
would need to be built before this becomes a one-command tool rather than a
manual checklist.

## Purpose

Use this recipe when triaging a batch of SWE/AI-Engineer-titled postings
(e.g. the output of `npm run ats:scan`, or a short hand-picked list) to decide
which ones are worth the time to apply to, specifically for a candidate who:

- needs H-1B sponsorship eventually (F-1, OPT not yet started),
- is targeting Software Engineer / AI Engineer / ML Engineer titles, and
- wants to avoid spending effort on roles that are both easy for AI to absorb
  *and* at companies with no real sponsorship track record.

It does not replace `recipes/scan.md` (posting discovery) or `recipes/apply.md`
(application drafting) — it sits between them: after a posting is found, before
time is spent on an application.

## Source Inventory

| Source | Path / Command | What it actually contains | Verified this session? |
|---|---|---|---|
| SOC/O*NET cognitive-demand table | `data/bls/compact/soc_occupation_compact.csv` | 1,017 O*NET-SOC rows, each with `cognitive_pivot_score` (avg. of selected reasoning/judgment/systems-analysis O*NET Level scores), job zone, OEWS wage/employment | Yes — read directly, confirmed real SOC codes and scores (table below) |
| H-1B sponsorship / funding by company | `data/80-days-to-stay/80-days-csv/mapped_student_employment_targets_v3.csv` | 30,369 companies with `Approval_Rate`, `median_salary_offered`, `top_job_titles_sponsored` (free-text list), funding stage/date | Yes — read directly, confirmed real Databricks row (below) |
| Role-quality / role-scorer combiner | `scripts/score/role-scorer.mjs` (`npm run score -- <roles.json> [--profile p.json] [--out-dir dir] [--md report.md]`) | Combines sponsorship/fit/role_quality votes × liveness/timeline gates into Apply/Consider/Skip, with a full audit trail per term | Yes — ran it twice this session (Worked Example) |
| Posting discovery / liveness | `npm run ats:scan -- --dry-run`, `npm run ats:liveness` | Greenhouse/Lever/Ashby postings, filtered by title/location, cross-checkable against the live provider API | Yes — ran `npm run ats:scan -- --dry-run` against Databricks earlier this session (892 found → 67 survived filters), cross-checked against `boards-api.greenhouse.io/v1/boards/databricks/jobs` directly |
| BLS source audit | `data/bls/BLS-audit.md` | Provenance + a stated known gap: "does not estimate employment trend slopes yet" | Yes — read directly |

**Real reference values pulled from `soc_occupation_compact.csv` this session**
(full-table `cognitive_pivot_score` range: min 1.611, max 4.877, n=894 of 1,017
rows scored):

| O*NET-SOC | Title | `cognitive_pivot_score` | Normalized 0–1 |
|---|---|---|---|
| 15-1221.00 | Computer and Information Research Scientists | 4.516 | 0.890 |
| 15-1299.08 | Computer Systems Engineers/Architects | 4.027 | 0.740 |
| 15-1211.00 | Computer Systems Analysts | 4.024 | 0.739 |
| 15-1252.00 | Software Developers | 3.834 | 0.681 |
| 15-1251.00 | Computer Programmers | 3.627 | 0.617 |
| 15-1254.00 | Web Developers | 3.584 | 0.604 |
| 15-2051.00 | Data Scientists | **blank — no score at all** | n/a |

The Data Scientists row is not a transcription error: every ability/skill/
`cognitive_pivot_score` field for `15-2051.00` is empty in the source CSV. A
title that gets mapped to this code cannot be scored by this recipe today.

## Proposed additions (not implemented — honest gaps, not hidden ones)

1. `[TODO: DEV] scripts/bls/title-to-soc-crosswalk.py` — does not exist
   anywhere in the repo (checked `scripts/` and `data/bls/`). Needed because
   `top_job_titles_sponsored` and real posting titles are free text ("AI
   Engineer", "Forward Deployed Engineer", "Solutions Architect"), while
   `soc_occupation_compact.csv` is indexed by O*NET-SOC code with only a
   canonical title + a sample of alternate titles. Without this script, every
   title-to-SOC mapping in this recipe is a manual, confidence-rated human
   judgment call (see Worked Example for a live example of how much that
   judgment can swing the result).
2. `[TODO: DEV]` a calibrated, non-zero `role_quality` weight in
   `scripts/score/role-scorer.mjs`. Today `CONFIG.weights.role_quality = 0.0`
   (the script's own comment marks it `[VERIFY]` / "UNPINNED"). This means
   the cognitive-pivot signal this recipe is built around can be fed into
   `role-scorer.mjs` and it will still contribute **nothing** to the Apply/
   Consider/Skip decision until a human picks a real weight — not something
   this recipe should silently decide (see `role-scorer.mjs`'s own `[VERIFY]`
   comment; this is their open question, not mine to resolve by editing the
   tested script).
3. `[TODO: DATA SOURCE]` SOC-level employment-trend slope. `BLS-audit.md`
   already states this is missing ("does not estimate employment trend slopes
   yet"). A static `cognitive_pivot_score` says nothing about whether an
   occupation's employment is already shrinking — that would need the
   2012–2024 OEWS national files already sitting in `data/bls/oesm24nat/`.

## What this recipe can verify

- Whether a specific O*NET-SOC code has a published `cognitive_pivot_score`,
  and what the number is (sourced, not estimated).
- Whether a target company appears in the 80-Days dataset with a nonzero
  H-1B approval history, what that rate is, and whether a given free-text
  title appears in its `top_job_titles_sponsored` list.
- Whether a posting is currently live (via `ats:scan` / `ats:liveness`,
  cross-checkable against the provider's own API).

## What this recipe cannot verify

- **Which SOC code a free-text title actually belongs to.** O*NET has no
  distinct code for "AI Engineer" — see the Worked Example, where one real
  posting title is given three equally plausible SOC mappings with
  materially different scores (0.681 / 0.890 / unscorable).
- **Whether company-level sponsorship history predicts this specific req.**
  `Approval_Rate` is historical and company-wide — a 99%+ rate says nothing
  about whether *this* team has H-1B budget *this* cycle.
- **Whether `cognitive_pivot_score` predicts future AI-substitution risk.**
  It is a snapshot of current O*NET task composition, not a forecast — using
  it as one is a modeling judgment, not a verified fact.
- **Whether any of this affects the Apply/Consider/Skip decision today.**
  Per the gap above, it does not, until `role_quality`'s weight is deliberately
  set above zero.

## Output Contract

### Agent output (machine-readable)
File: `data/role-scores.json` (written by `scripts/score/role-scorer.mjs` —
already a real, working output; this recipe does not invent a new schema).
Fields per role: `role_id`, `company`, `title`, `composite`, `recommendation`,
`reason`, `trace.votes[]` (factor, value, weight, contribution, source),
`trace.gates[]` (liveness, timeline), `trace.arithmetic`.

### Human report (readable)
File: `role-scores.md` (same script, `--md` flag). Reader: the student,
deciding where to spend the next hour of application effort. One row per
role: Role | Composite | Rec | Why | full audit string. Add one column this
recipe requires on top of the script's default table — **SOC mapping
confidence** (high/medium/low) — because the script has no field for it today;
until the crosswalk script exists, write it by hand next to each row before
trusting the recommendation.

## Stop Conditions

- Stop before treating a free-text title's SOC mapping as settled if
  confidence is "low" — get a second human opinion or skip scoring
  `role_quality` for that row entirely rather than guess.
- Stop before treating a company's historical `Approval_Rate` as a guarantee
  for a specific open req — flag it as company-level evidence only.
- Stop before reporting any Apply/Consider/Skip number as "cognitive-fit
  informed" unless `role_quality`'s weight was deliberately set above 0 and
  that choice is logged.

## RUN_LOG template

Per `logs/RUN_LOG.md`'s own header, students do not edit that file directly —
write entries to `logs/runs/<term>-<github-handle>-<n>.md` using this shape:

```markdown
## YYYY-MM-DD — cognitive-fit-swe run

- **Recipe:** cognitive-fit-swe v0.1.0
- **Inputs:** <roles.json path/shape>, <profile.json authorization string>
- **Commands run:** `npm run score -- <roles.json> --profile <profile.json> --md <out>.md`
- **Outputs:** Apply <n> · Consider <n> · Skip <n> (skip rate <n>%)
- **SOC mappings used:** <title → SOC code, confidence level, for each role>
- **Verified vs inferred:** <one line per vote: sponsorship/liveness = record; fit = model-judgment; role_quality = record (SOC lookup) but currently weight-0 unless patched>
- **Open issues:** <e.g. no crosswalk script yet; role_quality weight undecided; company-level approval rate ≠ this req's budget>
```

## Known related gap in the underlying engine

Not specific to this recipe, but discovered while building it and worth
flagging here per P6 (disagreement between recipe and script is a logged
defect, not something to quietly route around): `scripts/score/role-scorer.mjs`
computes a `role_quality` vote whenever one is supplied on a role record, but
`CONFIG.weights.role_quality` is hardcoded to `0.0`, so the vote is accepted,
shown in the audit trail with its real value, and then multiplied by zero. A
human reading only the summary line (`Apply 0 · Consider 4 · Skip 0`) would
have no way to know the cognitive-pivot signal never participated. See the
Worked Example for direct terminal output demonstrating this.
