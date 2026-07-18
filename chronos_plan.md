# CHRONOS
### The Adaptive, Study-Science-Driven Academic Operating System

> *CHRONOS never just tells you what to study — it tells you how, why that method fits you, and rebuilds your plan the moment your real behavior stops matching it.*

---

## 1. What CHRONOS actually is

Most "study apps" are task managers wearing an AI badge: a to-do list, a flashcard deck, maybe a chatbot bolted on the side. They don't know *why* a student keeps falling off their plan, and they give every student the same timetable shape regardless of who they are.

CHRONOS is different by design. It is built around a single core object — the **Learning DNA** — a living, evolving profile of exactly how one student learns: their chronotype, real attention span, subject-by-subject confidence and difficulty, distraction triggers, burnout risk, and which study methods actually work for *them*, evidenced by their own completion history. Every plan, every recommendation, and every intervention CHRONOS makes is read directly off that object. Nothing is templated, nothing is hardcoded, and no two students — even with identical exam dates — end up with the same plan.

**One-line pitch:** *CHRONOS is the study-science engine that builds a timetable no other student has — and rebuilds it the moment your real behavior tells it to.*

---

## 2. Why this isn't "just another ChatGPT wrapper"

This is the question every judge asks, and CHRONOS is architected to answer it concretely rather than rhetorically:

| Claim | How it's actually true here |
|---|---|
| **It's not a generic AI guess** | Every recommended method comes from a curated, structured knowledge base of ~20 named, evidence-based study techniques (spaced repetition, active recall, interleaving, the Feynman technique, Cornell notes, deliberate practice, and more), each tagged with attention/memory/energy requirements and retrieved on demand via a RAG pipeline (semantic embedding search with a deterministic keyword-overlap fallback) rather than baked into a static prompt. |
| **It's explainable, not a black box** | Every single plan block ships with a "why" — the specific profile signal, the method, and the reasoning that produced it. Every burnout/skip-risk/mastery prediction is a transparent, inspectable weighted combination of signals already visible in the student's own data, not a hidden model output. |
| **It works with zero API key** | Every AI-assisted feature (free-text onboarding, chat coaching, failure diagnosis, subject extraction) has a fully deterministic, offline fallback. Pull the network cable and the app still runs correctly end-to-end — this is enforced by an entire offline smoke-test suite, not a hope. |
| **It adapts from real behavior, not vibes** | The Learning DNA is never regenerated from a template. It is *mutated* in small, explained, auditable steps as real session outcomes come in — every change is logged with a timestamped reason, so the dashboard can say precisely why today's plan looks the way it does. |

---

## 3. The Learning DNA architecture

This is the single biggest engineering decision in the project: CHRONOS was deliberately re-architected from a static "form → timetable" generator into a full adaptive operating system built around one evolving object per student.

```
Adaptive Onboarding  →  Learning DNA  →  Plan Engine  →  Student behavior
   (conversation)         (the object       (reads DNA,     (completions,
                          every engine       nothing else)    skips, confidence)
                          reads from)              ↑                │
                                                    │                ▼
                                          Adaptive Engine  ←  Failure Engine
                                          (mutates DNA in       (diagnoses *why*
                                           small, explained      a session failed,
                                           steps)                 from free text)
```

**How a plan is actually built, end to end:**

1. **Adaptive onboarding conversation** (`onboarding_engine.py`) — a 20+ question diagnostic interview, branching intelligently via dependency rules (e.g. "what GPA are you targeting?" only appears if the student said GPA is a goal), that dynamically loops a full sub-interview per subject the student is taking. Every single answer is typed free text, never a picklist — "honestly I feel sharpest first thing in the morning" is interpreted, not selected, by a layered deterministic-then-LLM interpreter.
2. **Learning DNA generation** (`learning_dna.py`) — the interview answers are *derived* into structured traits, never asked directly. The student never sees a "chronotype" dropdown; they answer "when do you feel sharpest" and CHRONOS maps it. This produces version 1 of a rich, 25+ field profile: chronotype, focus span, distraction profile, stress tolerance, cognitive load tolerance, per-subject confidence/difficulty/interest/weight, and a preferred-methods seed.
3. **Dynamic method recommendation** (`method_kb.py`) — rather than a fixed subject→method lookup table, every one of ~20 study methods is scored live against the student's *current* DNA, the specific subject, and their current energy level — attention budget fit, memory-vs-recall balance, confidence level, past success rate with that method, and session-length fit all factor in. The ranking is recomputed every time, never cached statically.
4. **Plan generation** (`plan_engine.py`) — block count, subject ordering, session length, and method are all read purely from the DNA. A distractible, phone-flagged, exam-pressured early bird gets several short Pomodoro-flavored blocks in the morning; a focused night owl with nothing urgent gets fewer, longer deep-work blocks in the evening. Structurally different plans, not the same shape with different labels.
5. **Failure diagnosis, not a picklist** (`failure_engine.py`) — when a session is skipped, the student types *why*, in their own words. A seven-category cause taxonomy (attention leak, low-energy mistiming, passive engagement, unrealistic session length, cognitive overload, motivation collapse, external disruption) classifies free text into a stable cause, then hands back a long-form, study-science-grounded explanation of *why* that failure mode undermines learning and *why* the specific replacement approach fixes it — personalized to the student's own words when an LLM is available, and still genuinely substantive (not a one-liner) when it isn't.
6. **Adaptive engine — DNA that actually mutates** (`adaptive_engine.py`) — every logged outcome nudges the DNA a little (a completed session reinforces that method/time combination; a skip nudges preferred session length down). Separately, once a failure *pattern* repeats 3+ times, the DNA is permanently changed — a consistently avoided subject gets its scheduling priority raised and its failing method blacklisted; a time slot that keeps failing gets removed from the student's best study windows; repeated phone distraction becomes a standing risk factor that shrinks every future session. Every one of these mutations is appended to an auditable history log with a plain-language reason.
7. **Prediction engine** (`prediction_engine.py`) — burnout risk, tomorrow's skip likelihood, per-subject mastery, and exam readiness, all computed as transparent weighted combinations of DNA + logged history — every number the dashboard shows is explainable by design, not a mystery output.
8. **Personalized dashboard insights** (`dashboard_engine.py`) — natural-language insight cards ("Physics confidence climbed 2 points recently — current approach is working", "Consistency has dropped to 32% — worth revisiting the plan shape") computed fresh from *this specific student's* data. No stock templates filled in with a name.
9. **Conversational coach** (`chat_engine.py` / `chat_api.py`) — a free-text chat layer sitting on top of the same DNA: mention a new exam, a struggle, a distraction, or a study habit mid-conversation, and it's extracted and written straight into the Learning DNA, triggering a live plan regeneration when warranted.

---

## 4. Depth beyond the adaptive core

The system also ships a full academic operating layer built on top of (and interoperable with) the DNA engine:

- **RAG-based method retrieval** — a genuine retrieval pipeline over the study-method knowledge base: semantic embedding search when an embeddings-capable provider is configured, with a graceful, fully-functional keyword-overlap fallback when it isn't (e.g. running against a provider without an embeddings endpoint) — retrieval never breaks, it degrades gracefully.
- **Exam-driven backward planning** — given an exam date and topic list, CHRONOS works backward to allocate spaced review sessions automatically, instead of only planning one day at a time.
- **Pre-commitment / accountability rules** — students can set their own "if I skip 2 days in a row, do X" behavior-change rule at onboarding, evaluated automatically.
- **Weekly micro-retrospectives and evening reflections** — plain-language, personalized "what worked, what didn't, what to try next week" summaries, so the analytics read like coaching, not a stat dump.
- **Full account system** — password hashing, session tokens, multi-endpoint auth — this isn't a single hardcoded demo user hack; it's a real (if intentionally minimal) account layer.
- **Zero-downtime architecture migration** — the entire Learning DNA system was layered onto the existing app as an additive FastAPI router (`intelligence_api.router`, `chat_api.router`) rather than a rewrite: every legacy endpoint (`/generate-plan`, `/reflow`, `/dashboard`, `/analytics`, `/methods`, account/auth) continues to work untouched. This is a deliberate strangler-pattern migration, not a big-bang rewrite — the kind of decision a production engineering team, not just a hackathon team, would make.

---

## 5. Engineering practices that back up the claims

A project that talks about being "explainable" and "adaptive" only earns those words if it's actually tested that way. CHRONOS is:

- **Offline-first by construction.** Every AI-assisted code path (`ai_client.py`) is wrapped so a missing API key or a failed call *always* degrades to a deterministic heuristic — never raises, never blocks the student. This isn't a side note; it's proven by dedicated smoke tests that explicitly unset `OPENAI_API_KEY` and run the *entire* onboarding → plan → skip → pattern-escalation → dashboard flow successfully with zero network access.
- **End-to-end tested, not just unit-tested.** Three full smoke-test suites (`smoke_test.py`, `smoke_test_chat.py`, `smoke_test_freetext.py`) drive complete realistic user journeys through a live FastAPI `TestClient` — full onboarding conversations, three repeated skips to prove pattern-based DNA evolution actually fires, confidence logging, the conversational coach extracting and deduplicating subjects across turns, and chat history persisting in exact order across simulated page refreshes.
- **Backward compatible by design.** New engines were mounted as isolated routers precisely so the pre-existing profile-based flow keeps working for anyone mid-session on it — verified explicitly in the smoke tests ("legacy endpoints unaffected: OK").
- **Auditable by default.** Every mutation to the Learning DNA — big or small — is appended to a timestamped history log with a plain-language reason. Nothing about "why does my plan look like this" is ever a black box, even internally.

---

## 6. Tech stack

- **Backend:** Python, FastAPI, single-file SQLite-backed persistence (`storage.py`)
- **AI layer:** Pluggable LLM client (`ai_client.py`) supporting OpenAI-compatible and DeepSeek-compatible endpoints, with deterministic fallback logic mirrored across every consumer
- **Retrieval:** Embedding-based semantic search with keyword-overlap fallback (`method_kb.py` / RAG retrieval in `main.py`)
- **Frontend:** Single-file HTML/CSS/JS client (`index.html`)
- **Testing:** FastAPI `TestClient`-driven end-to-end smoke tests, including a fully offline suite

---

## 7. What's real today vs. the roadmap

In the spirit of the honesty the project itself was built around — no smoke and mirrors:

| Layer | Status |
|---|---|
| Learning DNA object, adaptive mutation, pattern escalation | **Built and tested end-to-end** |
| Free-text adaptive onboarding (backend) | **Built and tested end-to-end** |
| Dynamic method recommendation, RAG retrieval | **Built and tested end-to-end** |
| Failure diagnosis from free text, long-form explanations | **Built and tested end-to-end** |
| Predictions (burnout, skip risk, mastery, exam readiness) | **Built and tested end-to-end** |
| Conversational coach (chat → DNA → plan) | **Built and tested end-to-end** |
| Legacy account/auth, backward-planning, weekly retrospectives | **Built, working, backward-compatible** |
| Frontend chat/onboarding UI for the new adaptive conversation | **Backend fully supports it; frontend screen is the clear next step** |

---

*CHRONOS was built for the "education" track — not as a planner that manages tasks, but as a coach that understands the student behind them.*
