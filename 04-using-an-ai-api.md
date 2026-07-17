# Using an AI API safely

Registered participants will receive a free DeepSeek API key at kickoff on July 17, distributed through the participant channel rather than published in this repository. This file explains how to use that key, or any LLM API key, without leaking it and without exhausting the shared quota.

## The one rule: keys never appear in code

Automated scanners index every public GitHub push within minutes of it landing. A key that is committed once is compromised, even if you delete it in the next commit, because it remains in the git history and has already been captured. Follow this procedure from the start of the project:

1. Put the key in a file named `.env` at the project root:

```
DEEPSEEK_API_KEY=your-key-here
```

2. Add `.env` to your `.gitignore` before the first commit.

3. Commit a `.env.example` file containing the variable name with no value, so teammates and judges know which variables the project expects:

```
DEEPSEEK_API_KEY=
```

4. Load the key from the environment in code. Never type the key into a source file.

Python example. DeepSeek exposes an OpenAI-compatible API, so install the standard client with `pip install openai python-dotenv`:

```python
import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv()
client = OpenAI(
    api_key=os.getenv("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com",
)

response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": "You are a patient tutor. Explain simply, then ask one check question."},
        {"role": "user", "content": "Explain photosynthesis to a 12-year-old."},
    ],
)
print(response.choices[0].message.content)
```

JavaScript example, using `npm install openai dotenv`:

```javascript
import "dotenv/config";
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: process.env.DEEPSEEK_API_KEY,
  baseURL: "https://api.deepseek.com",
});

const response = await client.chat.completions.create({
  model: "deepseek-chat",
  messages: [
    { role: "system", content: "You are a patient tutor." },
    { role: "user", content: "Explain photosynthesis to a 12-year-old." },
  ],
});
console.log(response.choices[0].message.content);
```

If your project has a frontend, the API call must run on your server or through your hosting provider's environment configuration (Vercel, Render, and similar platforms all support environment variables). Any value shipped in browser code is public, because every user can open the developer tools and read it.

## Getting good output from the model

The system message is where most of your product's behavior lives. It defines the model's role, tone, and constraints once, and it applies to every request. Invest real time in it.

- Specify the output format exactly. "Reply with exactly 4 quiz questions as JSON in the shape [{question, options, answer}]" produces output you can parse and render. "Make a quiz" produces prose you cannot build a product on. Structured, parseable output is the difference between a chatbot and an application.
- Provide context in the request. Pass the student's level, their previous mistakes, or the actual course material into the prompt. The difference between generic output and personalized output is entirely determined by what you include in the request.
- Handle failure paths. The API will occasionally time out, return malformed JSON, or refuse a request. Catch these errors, retry once, and show the user a graceful message when the retry also fails.



#API DETAILS#

###This is not safe here, i know but for the sake of the hackathon, this will be left here for 2 days. 

DEEPSEEK_API_KEY=sk-95e4b71d1ac34180b5faf386c4d0a20a
DEEPSEEK_MODEL=deepseek-v4-flash
DEEPSEEK_API_BASE_URL=https://api.deepseek.com
## Do not waste the shared quota

The provided key is shared infrastructure for all participants. Cache responses during development instead of re-calling the API for input you have already tested, keep prompts as short as they can be while remaining specific, and never place an API call inside a code path that fires on every keystroke. If the key stops responding, report it to the organizers instead of retrying in a loop.
