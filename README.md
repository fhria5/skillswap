# SkillSwap — Trade Skills with Time Credits

You teach guitar. I teach English. No money changes hands — one hour of teaching earns one time credit, spent learning something else.

Offer a skill, find a match, book a session, confirm it, and the credit moves. Reviews build reputation, reputation improves future matches.

**Category:** Frankenstein (Kiroween 2026) — a skill marketplace, a time-credit ledger with escrow, a reputation system, and scheduling stitched into one app.

**Status:** Spec stage (14 Nov 2026). No app code yet. The build starts only after our first Kiroween app, FreshTrack, is declared perfect on a real phone.

## How it will work

1. Offer what you teach, list what you want to learn.
2. Matching engine scores pairs 0–100 (skill, schedule, reputation, responsiveness) and explains each score.
3. Book a session; the learner's credit is held in escrow.
4. Both sides confirm when done — the credit is released. Disputes hold it.
5. Reviews from finished sessions only build reputation.

## Live, not static

New offers, matches, and sessions appear in the feed without refresh, with motion throughout (transitions, credit progress, skeleton loading).

## Tech (planned)

- PWA (vanilla HTML/CSS/JS) on GitHub Pages.
- Cloudflare Worker + Workers KV for sync and the matching + ledger API.
- Built with Kiro: spec-driven development, steering, hooks, MCP server, property-based tests.

## Links

- Spec (Indonesian): [`SPEK.md`](SPEK.md)
- Phase plan: [`rencana-per-phase.md`](rencana-per-phase.md)
- Kiro spec: `.kiro/specs/skillswap/requirements.md`
