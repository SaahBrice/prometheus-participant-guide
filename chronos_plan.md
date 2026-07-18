# CHRONOS — Study-Science Operating System

An adaptive AI study coach: a chat-style onboarding builds a Student
Profile (chronotype, attention span, goal, failure pattern, learning
style, courses/subjects, and self-reported weak topics — all
student-input, not guessed), CHRONOS turns that into a daily
timetable matched against a study-methods knowledge base, and the
plan reflows in real time when you miss a session and tell it why.

The onboarding interview is 7 questions: 4 chip-based (chronotype,
attention span, failure pattern, goal) and 3 that pull in real
student input — learning style, the actual courses/subjects being
studied, and any topics the student already knows are shaky. The
weak-topics answer feeds directly into both retrieval (which study
methods get pulled for the day) and plan generation (that block's
"why" names the specific topic), instead of only ever inferring
weakness from confidence ratings after the fact.

## What's in this folder

- `index.html` — the full frontend (single file, no build step).
- `main.py` — FastAPI backend: profile storage, plan generation
  (LLM-backed with a deterministic rule-based fallback), reflow,
  session/confidence logging, and a weekly retro summary.
- `storage.py` — SQLite persistence layer (`chronos.db` is created
  automatically on first run).
- `requirements.txt`, `.env.example`

## Running it

```bash
pip install -r requirements.txt
cp .env.example .env        # optional — see below
uvicorn main:app --reload --port 8000
```

Then open `index.html` directly in a browser (double-click it, or
serve it with any static server). It talks to the API at
`http://localhost:8000` by default.

To point the frontend at a different backend (e.g. once you deploy
it), set `window.CHRONOS_API_BASE` before the page's script runs —
easiest way is a one-line `<script>` tag pasted just above the
closing `</head>`:

```html
<script>window.CHRONOS_API_BASE = 'https://your-api.example.com';</script>
```

### Optional: real LLM-generated plans

Without an API key, CHRONOS uses a deterministic rule-based planner —
fully functional, just less flexible than the LLM path. To turn the
LLM on, put your key in `.env`:

```
OPENAI_API_KEY=sk-...
```

The backend picks this up automatically and falls back to the
rule-based planner if the call ever fails, so a bad key or a flaky
network never takes the demo down.

### Locking down CORS

`CHRONOS_ALLOWED_ORIGINS` in `.env` defaults to `*`. Before sharing a
deployed link, set it to your actual frontend origin(s), comma
separated:

```
CHRONOS_ALLOWED_ORIGINS=https://your-frontend.example.com
```

## What's real vs. what's still a hackathon shortcut

The in-app Roadmap screen has the honest breakdown, but the short
version:

- **Real and working**: onboarding → profile → generated plan → tap a
  block for its "why" → simulate a missed session and watch the rest
  of the day reflow → mark blocks done → confidence check-ins → a
  retro screen that summarizes whatever's actually been logged (not
  canned copy). Data persists in SQLite across restarts.
- **Still single-user**: everything is stored under one profile, no
  accounts/auth yet — see the note at the bottom of `main.py` for the
  one-line path to multi-user.
- **Still a fixed knowledge base**: 10 study methods, hand-written,
  not a real vector-store RAG pipeline yet.

## Model roles ("Under the hood" card)

The Methods screen labels five conceptual roles the system splits work
across (Brain/reasoning, Memory/retrieval, Visual learning, a
second-opinion verification pass, and a Personalization engine tying
it all back to the student's logged behavior). The current build
actually runs the reasoning role on an OpenAI model via `OPENAI_API_KEY`
(deterministic fallback otherwise) — the card names the *kind* of
model each role is built to take, not a live integration with those
specific providers. Swapping `client = OpenAI(...)` in `main.py` for
another provider's SDK is the only code path that touches.

## API quick reference

| Endpoint | Method | Purpose |
|---|---|---|
| `/health` | GET | Liveness + whether the LLM path is enabled |
| `/methods` | GET | The study-methods knowledge base |
| `/profile` | GET/POST | Read/save the Student Profile |
| `/generate-plan` | POST | Generate today's 6-block plan |
| `/reflow` | POST | Regenerate the remaining day after a missed block |
| `/session/log` | POST | Log a block as completed or skipped (+reason) |
| `/session/today` | GET | Completed/skipped counts for today |
| `/confidence` | POST | Log a 1–5 confidence rating for a subject |
| `/retro` | GET | Plain-language summary built from the logs so far |
| `/reset` | POST | Wipe all stored data (handy for re-demoing onboarding) |