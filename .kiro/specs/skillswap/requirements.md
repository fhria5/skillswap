# SkillSwap — Requirements

Spec-driven requirements. No app code exists yet; this file is the contract the build must satisfy.

## REQ-1 — Offer / need skills

- User can offer skills to teach and list skills to learn: skill name + tag, level (beginner/intermediate/advanced), available slots, language.
- Description ≤ 2.000 chars; profile stored locally first, then synced when online.

## REQ-2 — Matching engine

- Every offer–need pair gets a score 0–100: skill 40, schedule overlap 30, reputation 20, responsiveness 10.
- Only pairs ≥ 50 are shown as matches, ranked highest first, with the reason for the score visible.
- Matching runs on the Worker; the client never trusts its own score.

## REQ-3 — Session booking + schedule

- User can request a session from a match: propose time slots; the other side accepts or rejects.
- Accepted sessions are scheduled with reminders; either side can cancel before start (credit never moves on cancel).

## REQ-4 — Time-credit escrow ledger

- 1 hour of session = 1 time credit. Credits are time only, never money.
- States: offered → matched → scheduled → in_progress → confirmed → settled / disputed → resolved.
- The learner's credit is held in escrow when a session is scheduled and released to the teacher only when both sides confirm completion.
- Ledger is double-entry: every credit movement records both sides (learner debit, teacher credit). The ledger must always balance.
- A dispute holds the credit; resolution returns it to one side with a recorded reason.

## REQ-5 — Confirmation + reviews + reputation

- Both sides confirm completion with one tap; a session cannot settle on one confirmation alone.
- Reviews are allowed only from finished sessions; reputation scores feed back into REQ-2 matching.
- False reviews (no finished session) are impossible by construction: review writes require a settled session id.

## REQ-6 — Live feed

- New offers, matches, and sessions appear in the feed without a manual refresh (polling ≤ 5 s).
- The feed shows what changed (new offer / new match / session confirmed), not a silent list.

## REQ-7 — Motion

- Cards transition on state change; credit progress animates; skeleton loading while fetching.
- `prefers-reduced-motion` is respected: animations become instant state changes.

## REQ-8 — Multi-device sync

- Workers KV sync, debounce 1,5 s, last-write-wins, cap 64 KB / 200 items (same pattern as FreshTrack).
- Offline: reading the list works; writes queue and sync on reconnect.

## REQ-9 — PWA + accessibility

- Installable PWA; contrast AA; tap targets ≥ 44 px; works at 320 px width.

## REQ-10 — Honesty

- Credits are time, never money; no real-money payments, no in-app video calls, no official ID verification.
- Scores are shown as computed, never as certainty ("likely match", not "perfect match").
- Seed data (12 skills + 6 sessions) is labeled as demo data, never presented as real users.
