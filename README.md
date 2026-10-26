# AI Agent Skills Collection

Skills help modify the behavior, reasoning, and communication patterns of AI agents to make them more effective, direct, and professional collaborators.

This repository features skills focused on communication quality and autonomous problem solving.

_Italian translations of the README, skills, and documentation are available in [`docs/it/`](docs/it/README.md)._

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

The skill guides the agent to:

- Maintain a dry, concise, and synthesis-oriented communication style.
- Remove wording that adds no value to mutual understanding.
- Preserve explanations, assumptions, and limitations when they are needed.

The criterion is useful synthesis, not the shortest possible answer. Exploratory sessions also showed differences in code and tool activity, so its influence should not be assumed to stop at prose.

**[Zero-Fluff Skill Documentation](docs/skills/ZERO_FLUFF.md)**

### 3. Solve Problem (`solve-problem`)

The **Solve Problem** skill provides a recursive decision-making procedure intended to improve how an agent chooses and applies actions without prescribing a particular implementation or removing its freedom of judgment.

The procedure guides the agent to:

- Define a falsifiable goal and specify domain concepts, contracts, and invariants.
- Verify the current state before deciding whether to change it.
- Decompose the problem when needed and integrate the results.
- Select actions by conceptual economy, not change size.
- Revisit the resulting state until the goal and requirements are satisfied.

For coding tasks, deterministic quality tools provide additional evidence and feedback. The skill guides judgment; it does not replace quality gates or guarantee correctness.

**[Solve Problem Skill Documentation](docs/skills/SOLVE_PROBLEM.md)**

---

## Project Structure

```text
.
├── docs/
│   ├── skills/
│   │   ├── NON_SYCOPHANTIC.md          # Detailed documentation & comparisons (English)
│   │   ├── SOLVE_PROBLEM.md            # Decision process, principles & limitations (English)
│   │   ├── ZERO_FLUFF.md               # Communication criterion, observations & limitations (English)
│   │   └── zero-fluff-pi-sessions/     # Recorded evaluation sessions (HTML)
│   └── it/                             # Italian localization
│       ├── README.md                   # Italian translation of the README
│       └── skills/
│           ├── NON_SYCOPHANTIC.md      # Italian translation of the skill documentation
│           ├── SOLVE_PROBLEM.md        # Italian translation of the skill documentation
│           ├── ZERO_FLUFF.md           # Italian translation of the skill documentation
│           ├── non-sycophantic/
│           │   └── SKILL.md            # Italian translation of the skill definition
│           ├── solve-problem/
│           │   └── SKILL.md            # Italian translation of the skill definition
│           └── zero-fluff/
│               └── SKILL.md            # Italian translation of the skill definition
└── skills/
    ├── non-sycophantic/
    │   └── SKILL.md                    # The skill definition & guidelines (English)
    ├── solve-problem/
    │   └── SKILL.md                    # Recursive problem-solving procedure (English)
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

Once placed, the agent harness can discover the skill. Loading behavior depends on the harness and the skill's configuration; discovery does not necessarily mean automatic activation.

`zero-fluff` and `solve-problem` are configured for explicit invocation (`disable-model-invocation: true`). In Pi:

```text
/skill:zero-fluff Who was Dante Alighieri?
/skill:solve-problem Develop an in-memory inventory reservation module.
```

See the [skill documentation](docs/skills/SOLVE_PROBLEM.md) for the decision process and its relationship to deterministic quality controls.

## License

This project is open-source and available under the MIT License.
