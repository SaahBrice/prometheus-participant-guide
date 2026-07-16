# Coding practices that matter

Judges will open your repository and read it. This file lists the habits that make a codebase look competent to a reviewer, particularly if this is your first serious project. None of them are difficult, and all of them are immediately visible to anyone reading your code.

## Naming

Names should state what a thing is or what it does, so that the code reads as a description of its own behavior.

- Use descriptive variable and function names rather than abbreviations: `student_score` instead of `ss`, and `generate_quiz(topic)` instead of `gq(t)`.
- Booleans should read as yes/no questions: `is_correct`, `has_submitted`.
- Functions are actions and should be named with verbs, such as `grade_essay`. Data is named with nouns, such as `essay_feedback`.
- Follow the standard convention of your language and apply it consistently: `snake_case` in Python, `camelCase` in JavaScript.
- If a variable needs a comment to explain what it holds, the name is wrong. Rename the variable instead of writing the comment.

## Structure

- Several small files, each with one responsibility, are easier to read and debug than one file that does everything. For a project of this size, a split such as `app.py` for the interface, `ai.py` for all API calls, and `data.py` for storage is sufficient.
- Keep functions short. If a function no longer fits on one screen, it is doing more than one job and should be split.
- Do not duplicate logic. The moment you paste the same block a second time, extract it into a function and call it from both places.
- Delete dead code and commented-out experiments before you submit. Reviewers notice them, and they signal a rushed or careless process.

## Comments and the README

Write comments that explain why, not what. A comment such as `x = x + 1  # increase x` adds nothing, whereas `# retry once because the API times out on long documents` records a decision the next reader cannot infer from the code.

The README is the first thing a judge reads, and it needs exactly four sections: what the project does, stated in two sentences; how to run it, as numbered steps that you have verified on a machine other than the one you developed on; what the AI does and which model or API you used; and who built it. A judge who cannot get your project running within five minutes will fall back to judging it from the video alone, and you lose the Technical Execution points that a working local run would have earned.

## Git

- Commit small changes frequently, with messages that describe the change: "add quiz scoring" rather than "update" or "fix stuff".
- Start committing from the first hour, not in one large commit at the end. A continuous commit history also serves as evidence that the work is your own.
- Create a `.gitignore` before your first commit. It must exclude your environment file (`.env`), dependency directories (`node_modules/`, `venv/`), and editor artifacts.
- Never commit API keys or passwords. This is covered in detail in [the API guide](04-using-an-ai-api.md), which you should read before writing your first API call. A key pushed to a public repository is compromised permanently, even if you remove it in the following commit, because automated scanners capture it within minutes and the value remains in the git history.

## Errors and stability

Assume that whatever can crash will crash during your recording. Wrap every API call in error handling and present the user with a readable message instead of a stack trace. Handle the empty input, the oversized input, and the API timeout, because those are the three failures that occur most often in live demonstrations. Under this scoring system, stability is worth more than an additional feature.
