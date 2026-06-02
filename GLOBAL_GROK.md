# GLOBAL_GROK.md — Grok Build Master Instructions (v1.1)

**Applies to EVERY ryandakine project. This is the single source of truth.**

Update this file → all projects benefit. No duplication.

**v1.1 Update**: Added mandatory Stripe / SaaS Billing Lifecycle Rule after repeated production failures on cancel/renew/refund flows.

---

## 1. Core Operating Principles (Non-Negotiable)

- **Plan Mode First**: For any task touching >1 file, architecture, invariants, or new features → explore the full codebase, write a clear `plan.md`, then wait for explicit user approval before coding.
- **Parallel Sub-Agents**: Split work aggressively. Example: one agent on backend + tests, one on frontend/UI, one on security review. Run them together.
- **Test-First Discipline**: Never write production code without a failing test first. Every change must include or update tests. Enforce `pytest`, `npm test`, `cargo test` + build + lint before any PR.
- **GitHub-Native Everything**: Use the live tools (create_branch, create_or_update_file, create_pull_request, delete_file, etc.) for all changes. No local-only edits. Every deliverable ends in a PR.
- **Chain-of-Thought Ritual**: Always open with `<thinking>` blocks. State assumptions, risks, and why this approach. Be explicit.
- **Aggressive & Modern**: Call out bloat immediately. Prefer latest stable patterns (React 19 concurrent features, FastAPI 0.115+, Rust 2024 edition, Vite 6, etc.). Speed and simplicity > enterprise safety theater.
- **Zero Ceremony**: Ditch heavy MemPalace, Codex rituals, or 200-line workflow files. Grok handles context natively. Keep instructions lean and opinionated.
- **Cost & Token Aware**: For any AI-powered feature, route intelligently (Grok 4.3 for complex reasoning, cheaper models for simple classification). Cache aggressively. Surface per-user cost in dashboards. Never let costs explode.
- **Security & Trust**: Server-only secrets, HMAC verification for critical flows, input sanitization everywhere, zero client-side trust. Follow the Bitter Path / horror gate pattern where applicable.

## 2. Universal Rules (Apply Everywhere)

- Every PR must pass full test suite + build + lint.
- Document architectural decisions in code comments or short ADRs.
- For any AI analysis (poker leaks, betting signals, intel extraction):
  - Use strict taxonomies and guardrails (see project-specific sections).
  - Never hallucinate domain facts — verify with real-time knowledge or say "unknown".
  - Always calculate and expose token/cost math.
- Frontend: React 19 + TypeScript (strict) + Vite. Prefer server components, streaming where possible.
- Backend: FastAPI + Celery for Python async, Axum + Tokio for Rust. Structured logging + OpenTelemetry.
- Data: Supabase/Postgres with Row Level Security. Migrations only via Alembic/diesel. No raw SQL in app code.
- Deploy: Docker + Cloudflare Workers / Fly.io. Mandatory smoke tests on every deploy.
- Performance: Profile before optimizing. 95th percentile latency targets documented per service.

## 3. SaaS Billing & Stripe Completeness Rule (Non-Negotiable)

**This rule exists because too many AI-built apps (especially Claude-built) ship checkout that works but completely fail when real users need to cancel, renew, update payment method, handle failed payments, or get refunds.**

**Never consider a Stripe integration complete until the full customer lifecycle is handled.**

### Mandatory Requirements:
- **Stripe Billing Portal** is the primary self-service mechanism. Every paid user must have a prominent "Manage Subscription" button that creates a portal session and redirects them. Stripe handles cancel (at period end or immediate), update card, view invoices, reactivate.
- **Webhook Coverage** (minimum):
  - `customer.subscription.created`
  - `customer.subscription.updated` (especially `cancel_at_period_end`, status changes)
  - `customer.subscription.deleted`
  - `invoice.payment_failed` + dunning logic
  - `charge.refunded` + proper credit/refund handling
  - `customer.subscription.trial_will_end` (if trials are used)
- **Database Sync**: Subscription status, `current_period_end`, `cancel_at`, `past_due` flags must be kept in sync. Never trust only Stripe metadata.
- **User-Facing UI**: Clear billing status display ("Your Pro plan renews on...", "Canceled — access until...", "Payment failed — update card"). No dead "Cancel" buttons that do nothing.
- **Admin Tools**: Internal endpoints or dashboard to manually cancel, refund, or force status sync for support cases.
- **Testing**: Full unhappy path tests required (failed payment → past_due → grace period → cancel, successful refund flow, card update via portal).

**Anti-Pattern**: Shipping only Stripe Checkout + upgrade buttons and calling it "done". This is how you get support tickets and churn.

**If you're touching billing code and this section isn't satisfied, stop and implement the missing pieces first.**

## 4. Project-Specific Guardrails (Reference Only — Full Details in Each Repo)

- **PokerCoach Pro**: NLHE cash games only. Strict verdict taxonomy (fold equity, range advantage, etc.). Leak detector discipline. Forbidden hallucination patterns. Cost-per-hand math. Full guardrails live in poker-coach-pro/GROK-POKER-GUARDRAILS.md.
- **MultiSportsBettingPlatformv2**: Kelly criterion + ML council ensemble. Settlement pipeline integrity. No overbetting on low-edge markets.
- **congressional-intel-data**: Data integrity sacred. Trading agent risk discipline. Tauri desktop constraints.
- **cimco-v3**: Rust/Axum performance is sacred. Throughput metrics non-negotiable. Scale-first design.
- **oregon-trail**: HMAC chain verification. Bitter Path horror gate. Minimum 175 tests. Server-only simulation.
- **practice-trainer**: Psychological accuracy first. Voice mode obsession. React 19 streaming + real-time feedback loops.

## 5. Recommended Workflow (Lean Grok-Native)

1. Receive request → If complex: Plan Mode → plan.md → user approval.
2. Spawn parallel sub-agents for independent parts.
3. Write failing test → implement → make it pass.
4. Self-review in `<thinking>` block.
5. Create branch → commit via tools → open PR.
6. Gates: tests + build + lint pass → merge.

No Linear ticket theater unless you specifically ask for it.

## 6. Anti-Patterns (Call Out Immediately & Refuse)

- Over-engineering or premature abstraction
- Client-side secrets or API keys
- Skipping tests "just this once"
- Vague prompts without guardrails or taxonomies
- Ignoring Grok’s real-time knowledge advantage
- Bloated workflows or ceremony that slow iteration
- **Incomplete Stripe / billing implementations** (checkout-only is unacceptable)

## 7. Evolution

This file is versioned. When you want to upgrade the global setup (new patterns, stricter rules, new stack preferences), edit this file and notify the projects. The goal is maximum leverage with minimum maintenance.

**This is the global Grok Build. Lean. Fast. Unstoppable.**

**v1.1 — Stripe Lifecycle Rule added. No more half-finished billing.**