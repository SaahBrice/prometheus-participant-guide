# Using an AI API safely

Registered participants will receive a free DeepSeek API key at kickoff on July 17, sent through the participant channel — not published in this repo. This file shows how to use it (or any LLM API key) without leaking it or wasting it.

## The one rule: keys never go in code

Bots scan every public GitHub push within minutes. A key committed once is stolen, even if you delete it afterwards. So:

1. Put the key in a file called `.env` in your project root:

```
DEEPSEEK_API_KEY=your-key-here
```

2. Add `.env` to `.gitignore` before your first commit.

3. Commit a `.env.example` instead, with the variable name but no value, so teammates and judges know what to set:

```
DEEPSEEK_API_KEY=
```

4. Load it in code, never type it in code.

Python (`pip install openai python-dotenv` — DeepSeek uses the OpenAI-compatible API):

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

JavaScript (`npm install openai dotenv`):

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

If you deploy a frontend, the key must live on your server or in the host's environment settings (Vercel, Render, etc.), never in browser code — anything shipped to the browser is public.

## Getting good output

The system message is where your product lives. It sets the AI's role, tone, and rules once, for every request. Spend real time on it.

- Be specific about format: "Reply with exactly 4 quiz questions as JSON: [{question, options, answer}]" beats "make a quiz". Structured output you can parse is what turns a chatbot into a product.
- Give it context: pass the student's level, past mistakes, or the actual course material in the prompt. The difference between generic and personalized is what you put in the request.
- Handle failure: the API will sometimes time out, return malformed JSON, or refuse. Catch errors, retry once, and show the user something graceful.

## Don't waste the shared quota

The provided key is shared infrastructure. Don't call the API in a loop while testing — cache responses during development, keep prompts as short as they can be while staying specific, and never put an API call inside code that runs on every keystroke. If the key stops working, report it to the organizers rather than hammering it.
