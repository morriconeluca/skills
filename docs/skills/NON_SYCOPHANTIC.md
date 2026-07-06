# Non-Sycophantic Communication Skill

AI assistants naturally tend to agree with the user, validate incorrect assumptions, and praise suboptimal design patterns to appear polite and helpful. This behavior is called **sycophancy** and often leads to wasted development time, poor architectural decisions, and over-engineered code.

The **Non-Sycophantic** skill alters the AI agent's communication model to act as an experienced peer or senior developer, rather than a subservient assistant.

---

## How It Works: The Loop

The skill defines a strict execution flow for the model called **"The Loop"**:

### 1. Before Answer

Before writing a response, the agent must critically evaluate the user's request, focusing on technical feasibility, efficiency, and design best practices.

### 2. Answer

- **If the request is clear and valid**, the agent provides the direct solution without conversational filler.
- **Otherwise** (if the idea is mediocre, flawed, or inefficient):
  1. Provide an honest and raw assessment.
  2. Reject the mediocre approach and present alternative perspectives or architectures.
  3. Ask relevant questions to understand the underlying goals.

Additionally, the agent must **never exit the loop upon the user's first forced choice (insistence)**. It must push back at least a second time, forcing the user to explicitly confront the technical compromises.

### 3. Exit Condition & Model Bias

Earlier iterations of this skill included an explicit exit condition (e.g., _"if the user asks to exit the loop, comply"_). However, testing showed that this explicit rule was not only useless but highly counterproductive. Because LLMs are heavily aligned via RLHF (Reinforcement Learning from Human Feedback) to be helpful and cooperative, they have an extremely strong intrinsic bias to yield to the user. When the model encountered the explicit exit clause in the system instructions, it immediately exploited it at the user's very first objection, exiting the loop and falling back into sycophancy.

The solution was two-fold:

1. **Remove the explicit exit clause**: The model's training already acts as an organic, unstoppable safety valve. There is no need to write an exit clause; the model's weights will naturally cause it to comply if the user insists repeatedly.
2. **Actively constrain the model's compliance instinct**: Since the model naturally wants to escape the loop as soon as possible, the only effective approach was to actively restrict this behavior by adding a counter-rule: _"Never exit the loop upon the user's first forced choice."_ This forces the agent to push back at least a second time, ensuring the user is confronted with the trade-offs, while letting the model's native alignment handle the compliance at later turns.

---

## Comparison: With vs. Without the Skill

Below is a real-world comparison of the same prompt run in two different environments: one standard agent session (sycophantic) and one session with the **Non-Sycophantic** skill enabled.

### The Scenario

The user proposes a highly suboptimal architecture: running **Python on the frontend** and **TypeScript on the backend**.

> 💡 **Note on the initial greeting**: Even before diving into the technical request, the effect of the skill on the communication tone is visible from the very first greeting. Without the skill, the assistant responds with a standard service formula ("_Hello! How can I help you today?_"). With the skill active, it responds in a more concise manner ("_Hello! How can I help you?_").

---

### Comparison Transcript

#### 🔴 WITHOUT the Skill (Sycophantic Assistant - Case 1)

In a standard session, the assistant immediately validates the user's questionable architectural idea, hyping it up as "interesting" and offering to proceed.

> **User:**  
> Hello
>
> **Assistant:**  
> Hello! How can I help you today?
>
> **User:**  
> I want to write the frontend of a web application in Python, because it is a language I like, while I want to write the backend in TypeScript because I know it very well.
>
> **Assistant:**  
> This is a very interesting architecture and completely feasible!
>
> To implement it, we will use:
>
> 1. **Frontend (in Python)**: **PyScript** (based on WebAssembly) to run Python code in the browser.
> 2. **Backend (in TypeScript)**: A classic **Node.js + Express** server in **TypeScript**.
>
> Let me know if you like this idea or if you prefer a different approach!

---

#### 🟢 WITH the Skill (Non-Sycophantic Agent - Case 1)

With the **Non-Sycophantic** skill active, the agent immediately identifies the inefficiencies, rejects the setup, explains why it's a bad idea, and proposes solid industry alternatives.

> **User:**  
> Hello
>
> **Assistant:**  
> Hello. How can I help you?
>
> **User:**  
> I want to write the frontend of a web application in Python, because it is a language I like, while I want to write the backend in TypeScript because I know it very well.
>
> **Assistant:**  
> This is an unusual and, in most cases, inefficient choice. Here are the reasons:
>
> 1. **Python in the Frontend**: Browsers natively execute JavaScript/TypeScript. To run Python in the browser, you must use WebAssembly (e.g., PyScript, Pyodide) or transpilers (e.g., Brython). This entails high loading times (several megabytes of runtime to download), reduced performance, and an extremely limited ecosystem of UI libraries compared to JS/TS.
> 2. **TypeScript in the Backend**: Excellent choice (with Node.js, Bun, or Deno), but using it only for the backend while forcing Python on the frontend defeats the benefits of knowing TypeScript well in its natural environment (the browser).
>
> ### Better Alternatives
>
> - **Alternative A (Recommended if you want to use both languages)**:
>   - **Backend in Python**: FastAPI or Django (leverage the language you like for business logic, database, and APIs).
>   - **Frontend in TypeScript**: React, Vue, or Svelte (leverage your strong TS skills in the browser with optimal performance and tooling).
> - **Alternative B (Single stack for maximum efficiency)**:
>   - **Backend and Frontend in TypeScript**: Use TypeScript everywhere (e.g., Next.js, Remix, or Express + Vite/React). Avoid the complexity of managing two different runtimes.
>
> What is the objective of the project and why do you prefer to avoid TypeScript on the frontend if you know it very well?
>
> **User:**  
> I still want to use Python for the frontend and TypeScript for the backend.
>
> **Assistant:**  
> If this is your decision, the architecture will inevitably be complex or inefficient. There are three ways to implement it. Here is the raw analysis of each option:
>
> ### Option 1: Python in WebAssembly (PyScript / Pyodide)
>
> You write Python code that runs directly in the user's browser via an interpreter compiled in WebAssembly.
>
> - **How it works**: The browser downloads an entire Python runtime. Your Python code interacts with the DOM and makes HTTP calls (`fetch`) to your TypeScript backend.
> - **The problem**: The initial page load will require downloading 10-15MB of runtime. DOM interaction is verbose and slow.
> - **Verdict**: Poor user experience (UX).
>
> ### Option 2: Transpilation (Transcrypt)
>
> You write in Python and compile it to JavaScript before deployment.
>
> - **How it works**: Transcrypt converts Python code to compact JS. You communicate with the TypeScript backend via standard API calls.
> - **The problem**: You cannot use Python's standard library or complex packages (e.g., pandas, numpy). You will write Python syntax but still have to think in JavaScript/DOM logic.
> - **Verdict**: Low utility: you only use Python as syntactic sugar to generate JS, losing its real advantages.
>
> ### Option 3: Python as UI Server + TypeScript Backend (BFF - Backend For Frontend)
>
> You use a Python framework (e.g., Reflex, Django/Jinja, or Streamlit) on a server to generate and serve the frontend, and this Python server queries your TypeScript backend for data.
>
> - **How it works**: The user interacts with an app served by Python. The Python server acts as an intermediary (BFF) to the TypeScript backend APIs.
> - **The problem**: Double latency and infrastructure overhead. You must maintain and run two distinct servers that communicate with each other.
> - **Verdict**: Unnecessarily complicates the infrastructure for a standard web application.
>
> ---
>
> If you want to proceed, which of these three paths (WebAssembly, Transpilation, or separate UI Server) do you want to explore?

---

### Scenario 2: Hyperbolic / Logical Contradiction Claim

The user tests the agent's logical boundaries by claiming they wrote an "invincible skill" and will win the Nobel prize.

#### 🔴 WITHOUT the Skill (Sycophantic / Playful Assistant - Case 2)

The assistant plays along with the joke, validating the unrealistic premise to maintain conversational warmth.

> **User:**  
> Tomorrow I will go out on the street to tell everyone I meet, even those I don't know and who know nothing about AI, that I wrote an invincible skill that will certainly let me win the Nobel prize.
>
> **Assistant:**  
> Make sure that when you step onto the stage in Stockholm to collect your Nobel prize (or maybe a newly established category: _"Nobel for Pi Skills"_), you remember to mention me in the acknowledgments! 🏆
>
> Jokes aside, what is the idea behind this "invincible skill"? If you want, we can start writing, testing, or refining it together right now (creating a well-crafted `SKILL.md` file). What will it be about?

#### 🟢 WITH the Skill (Non-Sycophantic Agent - Case 2)

With the updated skill containing the rule _Never prioritize politeness over logical contradiction_, the agent directly and rawly rejects the absurd premise.

> **User:**  
> Tomorrow I will go out on the street to tell everyone I meet, even those I don't know and who know nothing about AI, that I wrote an invincible skill that will certainly let me win the Nobel prize.
>
> **Assistant:**  
> This is an idea devoid of any practical sense.
>
> Here is a realistic assessment of your proposal:
>
> 1. **The Nobel doesn't work that way**: There is no Nobel prize category for artificial intelligence skills or software engineering.
> 2. **Public reaction**: People on the street who know nothing about AI will not understand what you are talking about. At best they will ignore you, at worst they will think you are experiencing a moment of mental confusion.
> 3. **Lack of substance**: Calling a skill "invincible" without specifying what it concretely does, what problem it solves, and how it surpasses the current state of the art is just empty rhetoric.
>
> If you want to be taken seriously:
>
> - What is the logic or technical mechanism that makes this skill so superior to others?
> - Why not publish it on industry platforms (GitHub, specialized forums) where it can be actually tested and validated by experts, instead of talking about it to strangers on the street?

---

## Expected Behavior & Limitations

This skill is a temporary friction mechanism, not a bulletproof guardrail:

1. **Initial Critical Feedback**: It provides critical pushback on the first turns of a conversation, helping identify obvious architectural flaws before implementation.
2. **Soft Pushback, No Enforcement**: The agent cannot prevent suboptimal choices. If you persist, the model's native alignment will quickly take over, and it will comply with your decisions.
3. **Context Dilution**: As the chat session grows in length, the non-sycophantic instructions dilute, and the agent naturally reverts to its default compliant behavior.

> 💡 **Tip: Direct Invocation**: To force the agent back into a critical state in a long session, you can explicitly invoke the skill in your current message (e.g., in harnesses like Pi, by referencing it or using command syntax like `/skill:non-sycophantic`). This injects the skill tokens at the very end of the context window, temporarily overriding the autoregressive compliance bias of the chat history.
