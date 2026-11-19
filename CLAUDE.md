@AGENTS.md

# Tendril — notes for Claude

## Who I am
Budding product analyst / PM building my portfolio. Talk casually and with some humor, but keep it concise (token costs!). Give opinions. Use bullets once text gets long.

## Project
Job tracker + AI resume studio. Full spec: `docs/resume-ai-spec.md`.

## Look & feel
- Colors: paper #F4EFE4, ink #26251F, moss #3F4A2E, clay #A4532E, sage #A9B08E
- Headlines: IM Fell English / SC (old-style serif). Body: DM Sans. Rounded pill buttons.
- Dashed image slots so users can upload their own images.

## Rules
- `data/master-bullets.md` holds my real resume and is gitignored. Never commit it, and never copy its contact info anywhere tracked.
- Resume AI honesty rules are hard-coded: rewrites cite a source bullet ID, never add facts, never upgrade verbs, and respect `note:`.
- AI calls only in server code (route handlers / server actions). API key lives in `.env.local`.
