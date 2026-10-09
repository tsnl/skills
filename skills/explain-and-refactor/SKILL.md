---
name: explain-and-refactor
description: Simplify code through independent behavioral explanations, pseudocode, critical debate, and verified refactoring. Use for explanation-first reviews or iterative clarity passes that make an implementation follow its conceptual model.
---

# Explain and refactor

Make the implementation a readable representation of the intended behavior.
Work within the requested files and interfaces, preserving earlier approved
behavior unless the user asks to change it. Carry the user's corrections, scope,
style constraints, and verification requirements through every pass.

## Obtain an independent explanation

Use a fresh explainer subagent for each simplification pass, with no inherited
conversation history. Continue debating with that agent within the pass; start
a new one for the next pass. The parent owns code inspection, edits, and checks.
The explainer must not inspect repository files, implementation, or diffs.

Use the host's delegation facilities and honor the user's model and reasoning
effort choices. This workflow does not require a particular provider or tool
name. If isolated delegation or a requested configuration is unavailable,
disclose that limitation. A local explanation-and-critique pass can still help
when permitted, but do not describe it as an independent review.

Give the explainer a neutral behavioral brief: purpose, inputs, observable
outputs, required semantics, representative examples, and relevant constraints.
Distinguish requirements from assumptions and known implementation limitations.
Do not include current helper names, internal decomposition, favored changes,
previous proposals, or earlier agents' conclusions. Even a behavioral summary
can bias the answer if it quietly prescribes the existing architecture.

Ask for a high-level explanation, then more detailed pseudocode, invariants,
edge cases, and unresolved assumptions. Each proposed stage or piece of state
should have an explainable purpose. The aim is the simplest sufficient model,
not a speculative redesign or a complete solution to adjacent problems.

## Debate the model

Treat the explanation as a hypothesis. Challenge it before adopting it:

- Trace concrete examples, including failures and boundary cases.
- Ask what breaks if a stage, representation, or piece of state is removed.
- Test ordering, identity, scope, lifetime, and error behavior where relevant.
- Challenge proposals that add generic machinery or merely move bookkeeping.

Let the explainer defend or revise its reasoning. Revise your own position when
the evidence warrants it; agreement alone does not establish correctness.

After the independent explanation, comparison with code may reveal facts needed
to assess a proposal. Introduce only the relevant facts and distinguish required
constraints from current design choices. This is informed critique, not another
independent explanation. Do not promote an incidental implementation choice into
a requirement for future agents.

Share the refined explanation before substantial edits. Ask the user when
intended behavior is materially unclear; routine comparison with the code is a
self-review step and does not require approval.

## Compare and simplify

Ask: **Does the implementation directly represent this explanation?**

Map each conceptual step to the code and its data. Look for places where a
reader must reconstruct the model from nested branches, repeated interpretation,
unrelated responsibilities, or state whose purpose cannot be explained locally.
For a mismatch, decide whether the explanation omitted an invariant, the code
contains unnecessary complexity, or behavior needs clarification. Neither the
existing code nor an elegant explanation automatically settles that question.

Refactor at those conceptual boundaries. Give helpers names that describe their
role in the explanation. When validation and execution separately interpret the
same input, consider parsing once into structured data that execution can
consume directly. Reuse existing representations when they already express the
needed distinctions. Introduce a record, flag, or abstraction only when it makes
the model clearer. Fewer lines or traversals do not by themselves establish a
simplification; separate straightforward passes may be easier to understand.

If the user requests a function-length target, measure formatted functions,
including local and anonymous functions. Split by responsibility rather than
compressing expressions onto fewer lines or adding numbered continuation
helpers. Apply line-width limits and other style requirements from the task;
do not turn one project's preferences into universal rules.

## Verify and repeat

Make a small, reviewable change and run relevant checks, exercising any affected
invariants. Update design notes within the authorized scope when they would
otherwise describe the old structure. A pass that justifies keeping the code is
also a useful result; do not manufacture edits to demonstrate progress.

Read the main operation again from its caller's perspective, then follow helpers
one level at a time. For another pass, brief a fresh explainer with the task
contract and verified behavioral facts, excluding previous proposed solutions.
Keep a short account of accepted changes and rejected alternatives in the parent
context so the loop does not repeatedly rediscover the same tradeoff.

Honor a requested single pass or explicit time/pass limit. When asked to iterate
to convergence, continue until the user stops or redirects the work, or until
no further useful simplification is supported by the review. Stop when the major
steps are visible in the code, helper responsibilities are coherent, and remaining
complexity serves concrete behavioral invariants. Do not keep cycling through
cosmetic variants or claim that simplification is mathematically impossible.

Report the useful conceptual changes, verification, and why the loop stopped.
Mention a rejected alternative when it explains necessary remaining complexity.
