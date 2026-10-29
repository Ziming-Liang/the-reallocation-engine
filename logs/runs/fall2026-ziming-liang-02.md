## 2026-10-29 — search/ layer built: resume.json, profile.yml, gaps.md (attested)

- **What was built:** `search/resume.json` (attested), `search/profile.yml`
  (attested), `search/gaps.md` (agent-drafted, student-edited). All three are
  gitignored per the repo's existing `/search/*` policy — nothing here is
  tracked or pushed.
- **resume.json — three attestation errors caught and fixed:**
  1. Dropped an unverifiable TA-experience mention (object-oriented design
     course TA while studying in the US) — no dates or employer given on the
     source resume, so it couldn't be attested; removed rather than guessed at.
  2. Agent's first-pass extraction silently dropped "Reactor" as the specific
     retry framework in the hybrid-retrieval-engine bullet (安科院 internship)
     — restored after re-checking against the source PDF.
  3. Agent's first-pass extraction silently dropped the "max 2 results per
     file" constraint in the code-search bullet (Devcode Agent project) —
     restored after re-checking against the source PDF.
  - Also explicitly challenged and confirmed (not changed): the "~25% hit-rate
    improvement" and "87%→95% accuracy" metrics — student confirmed both are
    personally defensible in an interview before they were kept.
- **profile.yml — field corrected from agent's first draft:** `stem_eligible`
  was drafted as `"uncertain — not DSO-confirmed"` per the assignment's own
  rule (don't assert a gate-critical field without confirmation); corrected to
  `true` after the student confirmed this was already confirmed with NEU's
  DSO/OGS, not just inferred from the major. Also resolved a real date
  conflict the agent flagged: resume PDF says graduation 2027-06, student's
  verbal answer first said 2027-05 — resolved to 2027-06 (resume is correct).
- **gaps.md — top gap:** named production Python experience. Evidence: a 2026
  AI Engineer JD template (korebpo.com) requiring 3+ years production Python
  (async, type hints, packaging), matching a pattern across a bank new-grad
  posting and a telecom posting found via web search. Rewritten in the
  student's own words in the file — plan is to build out this term's
  INFO7375 coursework project as the production-Python evidence artifact
  (pyproject.toml, module separation, pytest, README, one-command run),
  rather than a throwaway practice repo.
- **Killed row:** "Multi-agent / agentic workflow design experience" — killed
  because it was evidenced away by content already in resume.json (Devcode
  Agent's Planner/Worker/Reviewer architecture, Chorus's LangGraph4j workflow
  engine); the agent had flagged it purely from a JD-template keyword match
  without checking it against the student's own existing project evidence.

### Step 4 — Verification Check (honest answers)
- **resume.json traceable?** Mostly yes — every job/project entry maps to a
  named employer/repo with dates. Two internship bullets (安科院's "~25% hit
  rate" and Apexus-Tech's "87%→95% accuracy") rest on the student's word that
  they personally measured them; no external artifact (dashboard, paper,
  shared doc) was produced as corroborating evidence, only a direct
  confirmation in conversation. That's a real residual risk for an interview,
  not a fully closed loop.
- **profile.yml visa section — documents or hope?** `stem_eligible: true` is
  now based on an actual DSO/OGS confirmation per the student (not agent
  inference) and `needs_sponsorship: true` is an explicit student statement.
  However, `target_metros`, `open_to_remote`, `industry.target_sectors`, and
  `company_size_range` were agent-proposed guesses that the student accepted
  without adding independent specifics ("可以的" x3) — these read as
  convenient defaults rather than attested personal constraints, and are
  flagged as such directly in the file.
- **gaps.md evidence — real or invented?** Real: all three surviving gaps cite
  a specific, named source (a 2026 AI Engineer JD template + named postings
  found via live web search, with URLs), not training-data recall presented
  as a demand signal. The killed row is the one spot an invented-sounding
  demand signal was caught and removed before it did damage.
