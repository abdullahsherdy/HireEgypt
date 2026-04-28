# Junior Phase Playbook (Reusable)

Use this guide when you are new to a backend project and need a safe, repeatable way to deliver features.

## 1) Understand before coding

Read product scope, architecture, data model, and API workflows first. Your target is to explain the story in simple words:
- who does the action,
- what data changes,
- what success looks like,
- what can fail.

If you cannot explain this in 60 seconds, keep reading and asking questions.

## 2) Build in phases, not chaos

Follow this sequence:

1. **B0 Walking skeleton:** app + DB + tests running.
2. **B1 Auth:** register/login/refresh/logout/verify.
3. **B2 Jobs:** create, publish, search, close.
4. **B3 Applications:** apply, review, status changes, notifications.
5. **B4 Payments:** provider integration + webhook.
6. **B5 Realtime/background:** SignalR and scheduled/async jobs.
7. **B6 Admin/hardening:** approvals, limits, logging, audit.

Never skip B0. A stable foundation saves days later.

## 3) Per-story execution checklist (copy this every time)

- Understand business rules and edge cases.
- Add minimal Domain changes only for this story.
- Add Infrastructure changes (EF config, migration) only if needed.
- Add Application command/query + validator + handler.
- Add API endpoint + request/response + Problem Details.
- Add tests (happy path + at least one failure path).
- Update `DECISIONS.md` if you made a technical trade-off.

## 4) Move to next story only if gates pass

Do not continue until:
- build is green,
- tests are green,
- story works in Swagger,
- rules are enforced (not just happy path).

## 5) Interview-level framing

Use this format when answering interview questions:

1. **Context:** what problem you were solving.
2. **Decision:** what you implemented and why.
3. **Trade-off:** what you delayed or avoided.
4. **Result:** what improved (reliability, speed, clarity, safety).

Example:
"We delayed Redis until query latency justified it. This kept early complexity low, then we added cache when we had evidence from real query patterns."

## 6) Common junior mistakes to avoid

- Building all entities upfront "just in case".
- Skipping failure tests.
- Mixing responsibilities across layers.
- Adding infra tools before a story requires them.
- Starting new stories before finishing the current one.
