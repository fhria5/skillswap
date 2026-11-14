# SkillSwap — Product steering

## Product

SkillSwap trades skills with time credits. One flow: offer → match → book → confirm → credit moves → review.

## Audience

Anyone, anywhere, with one skill to teach and one skill to learn. UI in English.

## Non-negotiables

- No AI slop: disciplined palette, no stacked effects, honest copy. Scores are "likely matches", never certainty.
- Live, not static: the feed updates on its own; the UI moves (transitions, progress, skeleton). Motion respects `prefers-reduced-motion`.
- Credits are time, never money. No real-money payments, no in-app video calls, no official ID checks.
- Spec-driven: no feature without a requirement in `.kiro/specs/skillswap/requirements.md`.
- Tests before claims: every phase ends with a green regression run + a real-phone check by the user.
- Honest write-up: the Devpost write-up must say plainly which parts Kiro built and which were finished by hand (lesson from FreshTrack).
- AI is optional, never required: photo/description suggestions may use Verboo in a later phase; if time runs short, AI is cut first.
- AI Skill Finder links only, never installs: results are links + summaries; every ClawHub result shows its security-audit link (ClawHub had a malware campaign in 2026 — never auto-install a community skill).

## Tech

- Frontend: vanilla HTML/CSS/JS PWA on GitHub Pages.
- Backend: Cloudflare Worker (route on grokingstore.my.id) + Workers KV. Never workers.dev for a new Worker (known 404).
- Kiro: spec-driven development, steering, hooks (check-js-on-save), MCP server (read-only), property-based tests.
