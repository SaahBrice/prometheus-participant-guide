# Lumina Cameroon
## Judge Submission Guide

> **Built by Team Deucalion**  
> *World-class learning, built for Cameroon.*

---

## 1. Project Summary

**Lumina Cameroon** is a secure, curriculum-aware learning platform designed around the needs of students, teachers, and parents in Cameroon.

It is not a generic answer-generation chatbot. Lumina is designed to guide students through learning, support teachers with their own materials and class workflows, and give linked parents clear, privacy-safe progress context.

The platform is structured around the two Cameroonian education streams:

- **Anglophone:** English, GCE-aligned learning pathways
- **Francophone:** French, OBC/BAC-aligned learning pathways

The product foundation supports class-level locking, teacher-controlled learning materials, parent-child linking through Child Tracking Codes, private role-specific workspaces, and AI-assisted learning guidance.

---

## 2. The Problem

Learners, teachers, and parents often experience the learning journey separately.

### For students
- High-stakes examination preparation can be difficult without structured revision support.
- Access to relevant materials varies across schools and locations.
- Generic AI can give answers outside a learner’s class level or curriculum.
- Generic answer tools can undermine independent learning and academic integrity.

### For teachers
- Large classes make individual follow-up difficult.
- Teacher-created notes, rubrics, and resources are often disconnected from digital tools.
- Teachers need insight into misconceptions, submissions, and student learning activity.

### For parents
- Grades alone do not explain how a child is progressing.
- Parents need appropriate, privacy-safe information and practical ways to support study at home.

---

## 3. The Lumina Solution

Lumina connects the three most important people in a learner’s educational journey.

| User | Lumina experience |
|---|---|
| **Student** | Guided AI learning, classes, assignments, library resources, learning activities, progress, and revision support. |
| **Teacher** | Class management, curriculum materials, assignment tools, student insight signals, and source-aware AI support. |
| **Parent** | Secure child linking, progress summaries, deadlines, and suggested ways to support learning. |

### Core principle

> **Lumina helps students understand the work; it does not do the work for them.**

The AI is designed around a Socratic-first approach: it asks helpful questions, guides the method, and encourages the learner to think independently.

---

## 4. Key Features

### 4.1 Curriculum-aware AI Mentor

Lumina uses a protected server-side DeepSeek integration.

Before an AI response is produced, the platform is designed to:

1. Confirm that the user is authenticated.
2. Load the learner’s role, stream, class level, subject, and context.
3. Restrict the response to permitted curriculum scope.
4. Prioritise approved teacher materials where available.
5. Apply academic-integrity and student-safety rules.
6. Record the relevant source/fallback context for teacher oversight.

The AI policy includes:

- Socratic guidance before direct explanation
- No direct completion of tests, essays, or homework
- Class-level and language-stream alignment
- Cameroon-relevant examples where useful
- Student-safety escalation language
- No disclosure of private records, CTCs, API keys, or internal materials

---

### 4.2 Teacher Materials and Curriculum Context

Verified teachers can prepare materials for their classes, including:

- PDF documents
- Word documents
- PowerPoint presentations
- Plain text learning resources
- Revision guides
- Past-paper practice
- Rubrics and assignment briefs

Each resource is designed to be tagged by:

- Subject
- Class level
- Stream
- Term
- Visibility
- Rights status

Teacher materials can be either:

- **Student visible:** students may read or download the approved resource.
- **Internal only:** Lumina may use the material as teaching context, but students cannot download it.

---

### 4.3 Encarta-style Knowledge Library

The Knowledge Library provides a structured resource space for approved materials.

Students can browse resources relevant to their:

- Registered language stream
- Class level
- Subject
- Term

Planned library resource types include:

- Books and chapters
- Revision guides
- Past papers
- Question sets
- Marking schemes
- Articles
- Audio learning resources

The library architecture supports:

- Stream reading
- Approved offline downloads
- Resource summaries
- AI-guided discussion of permitted resource excerpts
- Flashcard generation
- Quiz generation
- Puzzle-game generation

At launch, the library is intended for **teacher-approved** and **verified open-licensed** content. Commercial textbooks require documented publisher permission or licensing before they are uploaded or distributed.

---

### 4.4 Learning Studio

The Learning Studio lets a learner turn permitted study material into active practice.

Supported activity types include:

| Activity | Purpose |
|---|---|
| **Flashcards** | Quick recall and spaced revision practice |
| **Mini-puzzles** | Matching, sequence, and concept-recognition learning games |
| **Practice quizzes** | Topic-focused self-assessment |
| **Teacher review** | A student can submit a completed activity for teacher feedback |

Learning activities are designed to retain:

- Activity type
- Student and class context
- Subject and topic
- Source resource where applicable
- AI feedback
- Teacher feedback
- Score
- Submission and review status

This supports a complete loop: **study → practise → self-check → submit → teacher feedback → improvement**.

---

### 4.5 Parent Child Tracking Codes

Each student receives a unique Child Tracking Code in the format:

```text
CTC-XXXXXX
```

This code allows an authorised parent to link to a child’s account during signup or after signing in.

Safety rules include:

- A parent can access only children linked to their own account.
- A maximum of two parent accounts may link to one student.
- A third linking attempt is rejected and audited.
- Teachers see CTCs only for learners enrolled in their own classes.
- Students see only their own code.
- CTCs are not sent to AI services or analytics tools.

---

### 4.6 User Preferences and Accessibility

Lumina includes the foundation for:

- English interface selection
- French interface selection
- Light theme
- Dark theme
- System theme
- Low-data mode preference
- Offline/download support roadmap

The interface preference is separate from a learner’s registered education stream. Teaching content remains aligned to the learner’s authorised English or French stream except when studying a language subject.

---

### 4.7 Secure Accounts

The platform includes account flows for:

- Account creation
- Email verification
- Verification callback and automatic session creation
- Sign in
- Password show/hide controls
- Password recovery by email
- Password reset
- Password change with current-password verification
- Logout
- Account deletion request with a retention/grace-period workflow

---

## 5. Security and Privacy Design

Lumina is designed around the principle that student data should be private by default.

### Role isolation

The system has distinct roles:

- Student
- Teacher
- Parent
- Administrator

Each role has a separate protected workspace.

| Route | Required role |
|---|---|
| `/dashboard` | Student |
| `/teacher` | Teacher |
| `/parent` | Parent |
| `/admin` | Administrator |

The database also uses **Supabase Row-Level Security**. Access is enforced at both the application layer and database layer.

### Administrator protection

There is no public default administrator password.

A predictable account such as `admin / admin1234` would allow an attacker to take over the platform. Instead, the initial administrator is provisioned through server-only environment variables or a manual approved setup process. Subsequent administrators should be created through a secure invite flow.

### API key protection

- DeepSeek API keys are server-only.
- Supabase service-role keys are server-only.
- Secrets are excluded from Git using `.gitignore`.
- No secret should be prefixed with `NEXT_PUBLIC_`.

---

## 6. Technology Stack

| Layer | Technology |
|---|---|
| Web application | Next.js 15, React, TypeScript |
| Database and Auth | Supabase PostgreSQL and Supabase Auth |
| Access control | Supabase Row-Level Security |
| File storage | Private Supabase Storage buckets |
| AI model provider | DeepSeek API through an OpenAI-compatible server-side client |
| Validation | Zod |
| Deployment | Vercel |

### Why this stack

This stack provides a low-operations foundation while keeping the important educational logic under Lumina’s control:

- Next.js supports responsive production web delivery.
- Supabase supplies authentication, database, storage, and RLS in one managed platform.
- DeepSeek is accessed only through the server, allowing model replacement later without changing the product experience.
- PostgreSQL and pgvector-ready structures support teacher-material retrieval and content-source tracking.

---

## 7. How to Run the Project

### Prerequisites

- Node.js 20 LTS or newer
- npm
- Supabase project
- DeepSeek API key

### Install

```bash
cd lumina-production
npm install
```

### Configure environment variables

Copy the template:

```bash
cp .env.example .env.local
```

Fill in the required values in `.env.local`:

```env
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

DEEPSEEK_API_KEY=
DEEPSEEK_MODEL=deepseek-v4-flash
DEEPSEEK_API_BASE_URL=https://api.deepseek.com

LIBRARY_STORAGE_BUCKET=library
ADMIN_BOOTSTRAP_EMAIL=
ADMIN_BOOTSTRAP_PASSWORD=
ADMIN_BOOTSTRAP_NAME=
```

Never commit `.env.local` to GitHub.

### Apply Supabase migrations

Run these files in order through **Supabase → SQL Editor**:

```text
supabase/migrations/001_initial_schema.sql
supabase/migrations/002_learning_features.sql
supabase/migrations/003_security_hardening.sql
supabase/migrations/004_library_preferences_learning_studio.sql
```

### Start development server

```bash
npm run dev
```

Open:

```text
http://localhost:3000
```

### Verify before deployment

```bash
npm run typecheck
npm run build
```

---

## 8. Important Demo Routes

| Route | Description |
|---|---|
| `/` | Public Lumina landing page |
| `/auth/signup` | Account creation |
| `/auth/login` | Standard account login |
| `/auth/recovery` | Password recovery |
| `/auth/admin` | Protected administrator login |
| `/dashboard` | Student workspace |
| `/dashboard/library` | Knowledge Library |
| `/dashboard/studio` | Flashcards, puzzles, and quiz creation space |
| `/dashboard/settings` | Password, logout, account deletion request, preferences guidance |
| `/teacher` | Teacher workspace |
| `/teacher/students` | Enrolled learner list and authorised CTC view |
| `/parent` | Parent workspace |

---

## 9. Honest Implementation Status

Lumina currently contains a validated secure product foundation, protected role-specific interfaces, AI guardrails, Supabase schema, RLS policies, library data structures, account flows, and presentation-ready interfaces.

The following require additional production implementation before onboarding real schools and learners at scale:

- Complete role-specific onboarding forms and server-side profile creation
- Material upload processing worker: virus scan, text extraction, content moderation, chunking, and indexing
- Private signed download URLs for approved library resources
- Full AI resource retrieval and book-specific chat/summarisation
- Learning activity generation and teacher grading workflow UI connected to live data
- Administrator invitation workflow
- Notification delivery
- Offline cache/service-worker delivery
- Safeguarding operational procedures
- Legal/privacy review and production penetration testing

This distinction is deliberate. Lumina does not claim features are complete until they have secure end-to-end workflows and real evidence from pilot use.

---

## 10. Pilot Plan

Lumina is designed to be validated through focused school pilots.

### Phase 1 — Classroom usefulness

- Recruit a small number of partner schools.
- Work with verified teacher champions.
- Begin with a high-need subject and class level.
- Validate teacher material workflow, guided learning, and assignment support.

### Phase 2 — Learning loop

- Add library resources and active-practice activities.
- Validate flashcards, quizzes, teacher review, and parent linking.
- Measure meaningful learning activity and teacher usefulness.

### Phase 3 — Responsible growth

- Improve low-data and offline access.
- Expand curriculum coverage.
- Add partner-school workflows and sustainable support operations.

---

## 11. Success Measures

Lumina measures meaningful activity rather than vanity metrics.

### Learner metrics

- Weekly successful learning actions
- Guided-session completion
- Assignment completion rate
- Topic-mastery progress
- Quiz improvement
- Learner confidence feedback

### Teacher metrics

- Teacher material usage
- Active teacher retention
- Time saved on repetitive support
- Student misconceptions surfaced
- Assignment/feedback turnaround

### Trust metrics

- Unauthorised-access incidents: target zero
- Safety-flag response time
- Academic-integrity support quality
- Parent-link success and security events
- AI source-logging coverage

---

## 12. Team

**Team Deucalion** is building Lumina Cameroon with the belief that educational technology should respect local curriculum, teacher expertise, learner privacy, and the realities of access in Cameroon.

> Lumina is not here to replace the teacher. It is here to make every learner better supported, every teacher better equipped, and every family better informed.

---

## 13. Submission Checklist

Before submitting or demonstrating:

- [ ] Run migrations 001–004 in Supabase.
- [ ] Create a private `library` Supabase Storage bucket.
- [ ] Configure `.env.local`; do not commit it.
- [ ] Configure DeepSeek and Supabase environment variables in Vercel.
- [ ] Add Supabase email callback URL.
- [ ] Run `npm run typecheck`.
- [ ] Run `npm run build`.
- [ ] Use synthetic/demo-safe data only.
- [ ] Do not claim users, revenue, school partnerships, outcomes, licences, or approvals that cannot be verified.
- [ ] Present the HTML pitch deck: `Lumina_Cameroon_Pitch.html`.

---

© Team Deucalion · Lumina Cameroon
