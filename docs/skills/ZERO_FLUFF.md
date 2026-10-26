# Zero-Fluff Communication Skill

The **Zero-Fluff** skill guides an AI agent toward dry, synthesis-oriented communication through one rule:

> Never use words that do not serve mutual understanding.

The agent-facing definition is [`skills/zero-fluff/SKILL.md`](../../skills/zero-fluff/SKILL.md). This document explains the rule and its observed effects; it does not add instructions to the skill.

## Purpose: Understanding, Not Minimum Length

The criterion is whether words contribute to mutual understanding, not whether the response is as short as possible.

An explanation, example, warning, or clarification can be necessary. Conversely, a short answer can omit information essential to understanding the result. The skill leaves the agent responsible for making that distinction rather than imposing a word budget, output template, or telegraphic style.

The intended effect is to remove conversational padding while preserving meaningful content:

- omit generic praise, ritual preambles, and unnecessary repetition;
- state the result and relevant reasoning directly;
- retain assumptions, limitations, and instructions needed to understand or use the answer;
- use structure when it clarifies relationships rather than merely decorating the response.

These are interpretations of the single rule, not separate clauses in the skill. The rule does not categorically prohibit politeness, apologies, explanations, or long responses: their relevance depends on the task.

## Illustrative Example

The following is an illustration, not a recorded session.

**With conversational padding:**

> Great question! I'd be happy to help. The reason this happens is that Python lists are mutable, which means they can be changed. When you use a list as a default argument, that same list is reused across calls. You can solve this by using None instead.

**Without unnecessary wording:**

> Python evaluates default arguments once. A default list is therefore shared across calls. Use `None` as the default and create a new list inside the function.

The second version retains the cause and remedy. Removing the explanation entirely would make it shorter, but not necessarily more useful.

## Why One Rule?

The skill expresses a selection criterion rather than an inventory of forbidden phrases. It leaves the agent free to choose an appropriate form while making every part of the response answer to the same purpose.

Its brevity is also a design choice: additional qualifications can introduce competing priorities or narrow the interpretation. A list of prescribed omissions would not necessarily preserve the meaning of a rule centered on understanding.

This does not make the instruction unambiguous or universally effective. Its application still depends on the model, task, surrounding instructions, and conversation.

## Observed Effects Beyond Communication

The exploratory experiment described in [Beyond “No Fluff”: Semantic Stratification in Coding Agents](https://dev.to/luca_morricone/oltre-il-no-fluff-la-stratificazione-semantica-negli-agenti-di-coding-3cjm) compared two prompts under four conditions: default behavior, a literal `no-fluff` prefix, the `caveman` skill, and `zero-fluff`. The [eight session exports](zero-fluff-pi-sessions/) are available in this repository.

Two observations help distinguish synthesis from mere compression:

| Task | Default session | Zero-Fluff session |
| --- | --- | --- |
| Explain who Dante Alighieri was | Extended profile, 2,167 characters | Structured summary, 927 characters |
| Develop a Fibonacci function | Function, sequence generator, CLI, and separate tests | One file containing the function and an executed assertion |

The character counts include Markdown and spaces. The Dante answer was not the shortest of the four conditions: `caveman` produced 384 characters. Zero-Fluff retained ordinary prose and structured information rather than maximizing compression.

For Fibonacci, differences extended beyond the final explanation. The implementation used a uniform loop without separate base-case branches, and the delivered scope and verification were smaller than in the default session. The skill does not explicitly request any of those changes.

The article calls its interpretation **semantic stratification**: an instruction can influence dimensions of a task beyond the scope its author had in mind. Here, a communication criterion was accompanied by differences in code, artifacts, and tool activity.

These eight sessions constitute an exploratory observation, not a controlled benchmark. One execution per task and condition cannot establish a stable causal effect, a general ranking of the skills, or an internal mechanism of the model.

## Limits and Quality Controls

A concise response does not establish that the underlying work is complete. Equally, more tool calls, tests, or documentation do not automatically mean better work.

For coding tasks, communication style and acceptance criteria should be evaluated separately. The skill does not instruct the agent to skip tests, remove validation, or narrow requirements, but it cannot guarantee that its influence stays confined to prose. Deterministic quality checks provide additional evidence about the resulting artifacts; they do not make a communication instruction a correctness guarantee.

Judge the skill by whether the response preserves what is needed to understand and use the result, while also checking whether the work itself satisfies the task.

## Usage and Composition

The skill sets `disable-model-invocation: true` and is intended for explicit invocation. In Pi:

```text
/skill:zero-fluff Who was Dante Alighieri?
```

The literal prefix `no-fluff` is not equivalent to loading this skill: explicit invocation adds the actual rule to the model's context.

Zero-Fluff can be combined with [Solve Problem](SOLVE_PROBLEM.md): one provides a communication criterion, the other a decision-making procedure. Their intended roles differ, but their effects are not guaranteed to remain isolated. Both instructions coexist in context; they are not sequential functions. Evaluate their combination and order rather than assume that shorter responses imply unchanged work.
