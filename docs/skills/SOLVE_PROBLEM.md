# Solve Problem Skill

The **Solve Problem** skill provides a recursive decision-making procedure intended to improve how an AI agent chooses and applies actions to solve a problem. It combines domain-oriented modeling, conceptual economy, and iterative verification without prescribing an implementation or removing the agent's freedom of judgment.

The agent-facing skill definition is [`skills/solve-problem/SKILL.md`](../../skills/solve-problem/SKILL.md). This document explains its design; it does not add instructions to the skill.

## Purpose: Guide Judgment, Not Replace It

Solving a problem requires more than executing checks. Someone must identify the relevant concepts, define their responsibilities, choose an approach, and decide what should be verified. Contracts and tests are often products of that work, not complete specifications available beforehand.

The skill addresses this decision-making layer. Its principles guide the agent in defining a solution and its acceptance criteria; feedback from verification then informs further decisions.

Originally developed for coding agents, the procedure uses general terms—problem, state, goal, concepts, actions—and is not restricted to software. Its usefulness outside coding remains to be evaluated.

## Modeling Principles

| Principle | Intended role |
| --- | --- |
| **Domain correspondence** | Make the solution's concepts and relationships reflect the problem, rather than accidental implementation details. |
| **Contracts and invariants** | Specify what each concept guarantees and which properties must hold. |
| **Responsibility boundaries** | Expose the contract while hiding the mechanism, so interactions do not depend unnecessarily on internal details. |
| **Precise naming** | Give concepts unique, exact names that express their meaning and boundaries. |
| **Conceptual economy** | Avoid or remove unnecessary concepts and indirections without confusing simplicity with a small edit. |

**“Conceptual economy, not change size” is an important distinction.** A tiny patch can preserve a confused model; a broader change can produce a simpler, more coherent solution. The skill leaves the agent responsible for evaluating what is necessary within the problem's constraints.

## How the Recursive Procedure Works

`Solve(problem, state)` operates on the current state, which may already contain a complete solution, an incomplete solution, or defects.

1. **Define the observable goal.** Establish a falsifiable `GoalTest`: an outcome that evidence could show has not been achieved.
2. **Identify and specify the concepts.** Define their contracts and invariants without ambiguity, and establish the modeling requirements that candidate actions must respect.
3. **Verify the current state.** Check the goal, observable contracts, domain invariants, and concept interactions. Falsifiable checks can fail against evidence; adversarial checks actively challenge the solution rather than merely demonstrate expected use.
4. **Return an adequate state unchanged.** If the checks pass and all requirements are satisfied, no intervention is required.
5. **Decompose when needed.** Split the problem along domain boundaries and apply the same procedure to each part, carrying the resulting state forward. Decomposition is conditional, not an obligation to invent more components.
6. **Choose and apply actions.** Generate candidates for unmet requirements, including integration of the parts. Select by conceptual economy and remove unnecessary concepts or indirections.
7. **Re-enter the procedure.** Apply `Solve` to the updated state, so the next verification determines whether further action is needed.

Recursion serves two purposes: solving smaller problems and revisiting the original problem after changes. Solving the parts alone does not establish that their integration satisfies the whole.

## Autonomy and Semantic Precision

The procedure specifies a decision structure, not a predetermined path. It does not prescribe a language, architecture, algorithm, test framework, number of alternatives, or amount of decomposition.

An open design choice is not automatically missing information that requires a question. Specifying a contract can be part of solving the problem, rather than a prerequisite the user must fulfill.

The wording is deliberately compact. The development approach builds on the hypothesis discussed in [Beyond “No Fluff”: Semantic Stratification in Coding Agents](https://dev.to/luca_morricone/oltre-il-no-fluff-la-stratificazione-semantica-negli-agenti-di-coding-3cjm): an instruction can influence more dimensions of an agent's work than its author intended. Adding a seemingly helpful qualification can redirect implementation choices, verification, or exploratory behavior.

For that reason, plausible failure modes are not by themselves sufficient reasons to add prescriptions. Changes should address observed shortcomings and be evaluated through repeated executions, rather than assume that more explicit text always improves behavior.

## Deterministic Quality Controls

For coding tasks, the skill is intended to work alongside deterministic quality tools, not replace them:

- compilers and type checkers;
- linters and static analysis;
- executable specifications and regression tests;
- property-based, integration, and mutation testing where appropriate.

The feedback loop is:

> **The skill guides decisions → tools produce evidence → the agent revises its decisions.**

A project can enforce quality gates on the result without predetermining the agent's implementation choices. The skill itself does not install tools, enforce gates, or guarantee that the agent executes every step.

Deterministic execution must also be distinguished from semantic adequacy: a test can reliably check the wrong property, and a passing suite can omit an important interaction. Tool feedback strengthens the process but does not guarantee a complete specification or an optimal result.

## Observations and Limits

Exploratory development sessions exercised three useful behaviors: constructing a solution from an empty state, preserving an already adequate solution, and correcting an introduced defect without weakening existing tests.

Other sessions showed variation in algorithms and edge-case coverage even with identical instructions. These observations are not a controlled benchmark and do not establish that the skill caused improvements over an agent operating without it.

The pseudocode is a natural-language instruction to an LLM, not a program executed by an interpreter. It does not guarantee termination, complete verification, faithful domain modeling, or adherence to every step. Its value should be assessed through observable choices, resulting artifacts, unnecessary changes, and the strength of independent checks—not merely the number of tests or tool calls.

## Usage

Load the skill explicitly for a problem. In Pi:

```text
/skill:solve-problem Develop an in-memory inventory reservation module.
```

The skill sets `disable-model-invocation: true`, so it is intended for explicit invocation rather than automatic activation by the model.

It can be combined with `zero-fluff`, but the instructions coexist in context rather than execute as a deterministic pipeline. Their interaction and order should be evaluated rather than assumed neutral.
