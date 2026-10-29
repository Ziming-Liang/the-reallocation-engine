## 2026-10-10 — cognitive-fit-swe: worked example (Databricks, real run)

- **Recipe:** cognitive-fit-swe v0.1.0 (recipes/cognitive-fit-swe.md)
- **Inputs:** hand-built roles.json (4 Databricks postings from a real
  ats:scan --dry-run this session) + profile.json (F-1, needs sponsorship
  post-June-2027); real values from data/bls/compact/soc_occupation_compact.csv
  and data/80-days-to-stay/80-days-csv/mapped_student_employment_targets_v3.csv
- **Commands:** `npm run ats:scan -- --dry-run`; `node scripts/score/role-scorer.mjs
  roles.json --profile profile.json --md role-scores-RUN-A-default-weights.md`;
  same against a scratch-patched copy with role_quality weight 0.20 (Run B);
  same against a 5-role set with one role_quality.p=null (deliberate break test)
- **Outputs:** Run A: Apply 0 · Consider 4 · Skip 0. Run B: same counts, composites
  up ~0.05–0.06 each, top-two gap narrowed 0.006 → 0.002. Break test: 5 roles
  scored, no crash, null vote silently dropped (not zeroed).
- **Result:** confirmed role_quality weight is hardcoded to 0 in the committed
  script (cognitive-pivot signal currently has zero effect on any
  recommendation); confirmed SOC 15-2051.00 (Data Scientists) has no
  cognitive_pivot_score in the compact table; confirmed "AI Engineer"-titled
  postings have no single correct SOC mapping (3 plausible codes, scores
  0.681 / 0.890 / unscorable).
- **Open issues:** no title→SOC crosswalk script exists; role_quality weight
  needs a human-calibrated value, not the demo 0.20 used here; company-level
  H-1B approval rate is not req-specific and this recipe has no way to check
  that distinction.
