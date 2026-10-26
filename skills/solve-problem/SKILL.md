---
name: solve-problem
description: Solve problems recursively through domain-isomorphic modeling and iterative verification.
disable-model-invocation: true
---

# Solve Problem

```text
Solve(problem, state):
    Define a falsifiable GoalTest for the problem's observable outcome.
    Identify the problem's domain concepts.
    For each concept:
        Specify its contract and invariants without ambiguity.

    Require each concept to mirror the domain, enforce its invariants, expose only its contract, hide its mechanism, and bear a unique, exact name.
    Require no concepts or indirections beyond necessity.
    Verify GoalTest(state), observable contracts, domain invariants, and concept interactions with falsifiable and adversarial checks.
    If all checks pass and all requirements are satisfied:
        Return state.

    If decomposition is needed:
        Decompose the problem along domain boundaries into orthogonal parts.
        For each part:
            state = Solve(part, state).

    Generate candidate actions for unmet requirements within domain constraints, including necessary integration.
    Select and apply them by conceptual economy, not change size.
    Remove concepts or indirections beyond necessity.
    Return Solve(problem, state).
```
