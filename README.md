# 🌱 Tendril

A job-search tracker with an AI resume studio. Paste a job description and Tendril checks your fit, picks the best bullets from your master list, suggests rewrites, and flags any rewrite that tries to invent experience.

> Portfolio project, built in public.

## Features (planned)
- **Home:** weekly/monthly goals, progress cards, pomodoro timer
- **Job tracker:** status pipeline (Applied → Reached out → Interviewing → Rejected / Ghosted)
- **Networking:** informational interviews, recruiters, follow-ups
- **Resume studio:** JD → fit check → bullet picker → rewrites → keyword match → human review → export
- **Settings:** goals, resume length, master bullet upload

## How the resume AI stays honest
- It's a fixed pipeline of small AI calls, not a free-roaming agent.
- Every rewrite has to point to a source bullet ID.
- A code-level validator blocks any new number, tool, company or title that isn't in the source bullet.
- Each bullet can carry private `note:` caveats (scope limits, overlaps) that the AI must respect.

See [`docs/resume-ai-spec.md`](docs/resume-ai-spec.md).

## Stack
Next.js (App Router) · TypeScript · Tailwind · Zod · Claude API

## Run locally
```bash
npm install
cp data/master-bullets.example.md data/master-bullets.md   # then add your real bullets
npm run dev
```
`data/master-bullets.md` is gitignored, so your contact info never gets committed.
