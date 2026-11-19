# Resume AI — spec v1 (Nov 19, 2026)

## Decisions
- **Architecture:** fixed pipeline of small AI calls with structured (JSON) output, not a free-roaming agent. Each step is testable alone and maps to a Resume studio UI step.
- **Stack:** Next.js + TypeScript. AI calls run in server-side API routes, so the API key never reaches the browser. Schema validation with Zod.
- **Resume length:** user setting in the Settings tab (1 page / 2 pages / max bullets per role). The bullet picker uses it as its budget.
- **Cover letters:** out of scope for v1.
- **Master resume source:** tagged markdown (format below), upload/replace only.

## Pipeline
| # | Step | Input → Output | How |
|---|------|----------------|-----|
| 1 | Parse JD | pasted text → `{title, company, seniority, must_haves[], nice_to_haves[], keywords[]}` | Small/fast model, structured output |
| 2 | Fit check | requirements + master bullets → per-requirement evidence (`covered` / `partial` / `none` + bullet IDs) | AI matches evidence; **code** computes score (must-haves weighted ~2× nice-to-haves) |
| 3 | Pick bullets | ranked bullets + length setting → selected bullet IDs per role | AI ranks, code enforces the length budget and overlap rules |
| 4 | Suggest rewrites | selected bullets + JD language → rewrites, each tied to a source ID | Stronger model; reword/reorder only; must respect each bullet's `note:` |
| 5 | Keyword match | JD keywords vs final bullets → `covered` / `covered by rewrite` / `missing` | Code first (exact + synonym map), AI for semantic matches |
| 6 | Review → export | originals vs rewrites → approved resume | Side-by-side diff + human checklist; nothing exports until every rewrite is approved |

## Master bullet format
```
## Org | Title | Dates | Location        ← "?" when unknown (parser flags these)
link: https://...                          ← optional, entry-level
- [pa01] Bullet text exactly as written
  tags: sql, retention, stakeholder
  note: PRIVATE caveat for the AI, never printed
  link: https://...                        ← optional, bullet-level
  src: cv                                  ← optional provenance
```
- Sections: CONTACT, EDUCATION, EXPERIENCE, LEADERSHIP & ACTIVITIES, PROJECTS, CERTIFICATIONS, SKILLS.
- IDs are role-prefixed (`pa`, `mi`, `gs`…) so adding a bullet never renumbers others. IDs never change once assigned.
- Parsed to JSON on upload; the user confirms a preview before saving.

### `note:` field (private AI context)
- Fed to the picker and rewriter, never rendered on a resume.
- Used for scope caveats, verb limits ("co-designed ≠ led"), and **overlaps** ("same metric as pa01, never use both").
- Overlaps are also enforced in code: the picker rejects a selection containing both bullets of an overlap pair.

## Honesty guardrails (hard-coded, not editable)
- Every rewrite must reference a source bullet ID, or it's rejected.
- Post-rewrite validator (code, not prompt): flags any number, tool, company, or title not present in the source bullet → blocked until the user approves.
- The AI may reword, reorder, and change tense. It may not add facts or upgrade ownership verbs.

## Build order
1. Master bullet parser + format template
2. Step 1: JD parser
3. Steps 2–3: fit check + picker
4. Steps 4–5: rewrites + validator + keyword match
5. Step 6: review UI + export

## Parked
- Cover letters
- Direct Google Docs import
