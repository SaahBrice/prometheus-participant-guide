# Coding practices that matter

Judges open your repo. This file is the shortlist of habits that make a codebase look competent, especially if this is your first project. None of it is hard; all of it is visible.

## Naming

Names should say what a thing is or does, so the code reads like a sentence.

- Variables and functions: descriptive, not abbreviated. `student_score` not `ss`. `generate_quiz(topic)` not `gq(t)`.
- Booleans read as questions: `is_correct`, `has_submitted`.
- Functions are verbs (`grade_essay`), data is nouns (`essay_feedback`).
- Pick one convention per language and stick to it: `snake_case` in Python, `camelCase` in JavaScript.
- If you need a comment to explain what a variable holds, the name is wrong. Rename it instead.

## Structure

- One file doing one job beats one 800-line file doing everything. A simple split like `app.py`, `ai.py` (all your API calls), and `data.py` is enough for a hackathon.
- Keep functions short — if a function doesn't fit on your screen, it's doing too many jobs.
- Don't repeat code. If you paste the same block a third time, turn it into a function.
- Delete dead code and commented-out experiments before submitting. Judges see them.

## Comments and the README

Comment the why, not the what. `x = x + 1  # increase x` is noise. `# retry once because the API times out on long documents` is information.

Your README is the first thing a judge reads. It needs exactly four things: what the project does in two sentences, how to run it step by step (test these steps on a clean machine or a friend's laptop), what the AI does and which model/API you used, and who built it. A judge who cannot run your project in five minutes will judge it from the video alone.

## Git

- Commit small and often, with messages that say what changed: "add quiz scoring" not "update" or "fix stuff".
- Commit from day one, not in one giant lump at the end. A real commit history also proves the work is yours.
- Add a `.gitignore` before your first commit. It must include your environment file (`.env`), dependency folders (`node_modules/`, `venv/`), and editor junk.
- Never commit API keys or passwords. This is covered fully in [the API guide](04-using-an-ai-api.md) — read it before you write your first API call, because a key pushed to GitHub even once is compromised forever, even if you delete it in the next commit.

## Errors and stability

The demo gods are cruel: whatever can crash, will crash while recording. Wrap your API calls in error handling and show the user a friendly message instead of a stack trace. Handle the empty input, the too-long input, and the API timeout. Stability is worth more points than an extra feature.
