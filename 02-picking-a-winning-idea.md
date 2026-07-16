# Picking a winning idea

The formula for a two-day hackathon is narrow by necessity: one user, one problem, one working flow. You do not have time to build a platform, and the judges are not expecting one. You have time to build one thing that works every time it is demonstrated, and that clearly helps a specific person learn or teach.

## A test to run before you write any code

Answer each of the following in a single sentence. If you cannot, the idea is not ready to build.

- Who exactly uses this? "Students" is not exact. "A secondary school student revising for final exams alone at home" is exact.
- What can that person do after using your tool that they could not do before?
- What is the AI doing that a conventional application without AI could not do?
- Can you demonstrate the entire flow in under 90 seconds?

## Directions that fit this hackathon well

These are starting points rather than assignments. The winning team will be the one that picks a narrow slice of one of these and executes it completely.

Agentic AI in education is the richest territory available right now. The distinction is this: a chatbot answers the question it was asked, while an agent pursues a goal across multiple steps without being prompted at each step. Examples that fit a two-day scope include a study coach that generates a revision plan, quizzes the student, detects weak topics from the answers, and adjusts the plan accordingly; a teaching assistant that takes a syllabus as input and produces lesson plans, exercises, and marking schemes; or a feedback agent that reads a student's essay, identifies the recurring structural weakness, and generates targeted practice for exactly that weakness.

Personalized learning covers tools that adapt pace, difficulty, or explanation style to the individual. In these products, the system's memory of the learner is the actual product, and the LLM is the mechanism that acts on it.

Accessibility covers converting lectures into structured notes for deaf students, describing diagrams for blind students, simplifying dense academic text for younger readers or non-native speakers, and voice-first interfaces for users who cannot rely on reading.

Teacher-facing tools are consistently underrated by participants. Teachers lose hours daily to grading and lesson preparation, so a tool that measurably saves a teacher one hour per day produces exactly the kind of demonstrable value that the Educational Impact criterion rewards.

Underserved contexts cover learning in languages that the major platforms ignore, preparation for national curricula and exams, and tools designed to function on low-end phones or unreliable connections. Judges reward teams that clearly understand a context that other teams overlooked.

## Smart use of AI, in the product and in the process

In the product: do not ship a thin wrapper around a chat interface. Combine the LLM with structure that you build yourself, such as a question bank, a spaced-repetition schedule, retrieval over real course material, or a grading rubric. The structure is what makes the product yours; the LLM is what makes it adaptive.

In the process: using AI coding assistants is permitted and sensible, but you must understand every line you submit. Judges read the code and may ask you questions about it. Pasted code that you cannot explain, debug, or modify will fail under the Technical Execution criterion at exactly the moment you need it to hold up. Use AI to move faster through work you understand, not to skip the understanding.

## Scope discipline

Write your full feature list at the start of day one, then cut it to the minimum set that demonstrates the core idea. When you are tempted to add a feature midway through, apply this test: does the feature appear in the 2-minute video? If it does not, it does not exist as far as the judges are concerned. Finishing early and spending the recovered time on stability and the pitch is the trade that wins hackathons.
