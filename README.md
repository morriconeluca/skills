# AI Agent Skills Collection

Skills help modify the behavior, reasoning, and communication patterns of AI agents to make them more effective, direct, and professional collaborators.

Currently, this repository features core skills focused on communication quality.

_Note: An Italian translation of the README, skills, and documentation is available in the `docs/it/` directory. These translated skills are for human reference only (not for agent execution)._

---

## Featured Skills

### 1. Non-Sycophantic Communication (`non-sycophantic`)

AI assistants naturally tend to agree with the user's choices, even when those choices are suboptimal, inefficient, or technically flawed (a phenomenon known as AI sycophancy).

The **Non-Sycophantic** skill forces the agent to:

- Maintain a peer-to-peer (equal collaborator) tone.
- Critically evaluate user requests before answering.
- Provide an honest and raw assessment, reject mediocre ideas, and propose alternative perspectives.
- Ask relevant questions when necessary.
- Resist user insistence without prematurely backing down (never exit the loop upon the user's first and second forced choice).
- Never prioritize politeness over logical contradiction.

To see how this skill works in practice, view the detailed documentation:
👉 **[Non-Sycophantic Skill Documentation](docs/skills/NON_SYCOPHANTIC.md)**

### 2. Zero-Fluff Communication (`zero-fluff`)

AI assistants naturally tend to introduce conversational filler, polite preambles, apologies, and wordy explanations that add no semantic value.

The **Zero-Fluff** skill enforces synthesis-oriented communication governed by a single imperative rule:

> _"Never use words that do not serve mutual understanding."_

The skill forces the agent to:

- Maintain a dry, concise, and synthesis-oriented communication style.
- Strip away pleasantries, apologies, filler phrases, and conversational padding.
- Maximize information density and focus strictly on mutual understanding.

---

## Project Structure

```text
.
├── docs/
│   ├── skills/
│   │   ├── NON_SYCOPHANTIC.md          # Detailed documentation & comparisons (English)
│   │   └── zero-fluff-pi-sessions/     # Recorded evaluation sessions (HTML)
│   └── it/                             # Italian localization (reference only)
│       ├── README.md                   # Italian translation of the README
│       └── skills/
│           ├── NON_SYCOPHANTIC.md      # Italian translation of the skill documentation
│           ├── non-sycophantic/
│           │   └── SKILL.md            # Italian translation of the skill definition
│           └── zero-fluff/
│               └── SKILL.md            # Italian translation of the skill definition
└── skills/
    ├── non-sycophantic/
    │   └── SKILL.md                    # The skill definition & guidelines (English)
    └── zero-fluff/
        └── SKILL.md                    # The skill definition & guidelines (English)
```

---

## Skill Design Principles

When writing or contributing skills to this collection, follow these core principles:

- **Keep it Brief**: Keep instructions short. Long texts add noise and reduce the model's focus on key directives.
- **Imperative & Unambiguous Language**: Write direct, imperative instructions. Clear commands (e.g., "Never do X") yield more consistent results than passive suggestions.
- **De-center the Human Perspective**: Do not write to the model as if it were a human colleague. Eliminate conversational padding, emotional framing, and explanations. Focus only on rules that directly constrain the output.
- **Semantic Precision**: Use precise words rather than long explanations. A single exact term can trigger complex behaviors. However, choose words carefully: each word can introduce unwanted side-effects or unintended interpretations.
- **Structured Logic**: Use logical structures like If/Else conditions, bullet points, or numbered sequences. Models process causal relationships better in structured formats than in verbose paragraphs.
- **Acknowledge Native Model Priors**: Do not fight the model's default alignment (RLHF). You cannot completely overwrite its training. Instead of writing rules to bypass this alignment, constrain it. For details on this approach, see the [non-sycophantic skill documentation](docs/skills/NON_SYCOPHANTIC.md#3-exit-condition--model-bias) where the model is prevented from yielding to the user by forcing it to push back against forced choices (`Never exit... on first and second forced choice`).
- **Design for Non-Determinism**: Expect non-deterministic outputs. Designing a skill means setting behavioral boundaries and guardrails, not expecting a single, fixed path of response.

---

## How to Use Skills

AI agent harnesses (such as [Pi](https://pi.dev/)) can load skills on-demand.

I strongly recommend using **Pi** for software engineers who want maximum control over their AI agents. Custom skills added to the context perform best with tools that feature a succinct, near-zero system prompt. In contrast, heavier agent frameworks with large, highly structured system prompts are prone to instruction conflict, and the custom skills must compete with existing noise in the context.

### Installation / Setup

Copy the desired skill directory to one of the following locations in your project or global environment:

- **Project-level**: `.agents/skills/` or `.pi/skills/`
- **Global**: `~/.agents/skills/` or `~/.pi/agent/skills/`

Once placed, the agent harness will automatically discover the skill description and load the full guidelines when a matching task is detected.

## License

This project is open-source and available under the MIT License.
