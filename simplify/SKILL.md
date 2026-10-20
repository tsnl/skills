---
name: simplify
description: Simplify recently written modules lossily and from the top down, using an independent behavioral explanation, a decision about which behavior is essential, debate, and verified refactoring. Whole-program simplification requires explicit user authorization.
---

# Simplify

Make the code a direct, readable representation of the behavior that matters.

- **Lossy.** The best simplification removes behavior, cases, options, and
  generality that do not serve the user, rather than expressing them more
  neatly. Keep what is essential; cut what is incidental; report every cut.
- **Top-down.** A stage, protocol, representation, or piece of stored state
  that need not exist takes every function that maintains it with it. Trim
  those first, then modules, then functions.

## Scope

By default, work on a handful of recently written modules; state the set
before editing, and ask if it is unclear. Whole-program simplification needs
explicit user authorization. Repeating passes does not widen the set. Read
other code as needed, but edit only within the scope; describe changes that
need more and ask first.

## Essential and incidental behavior

- **Essential:** stated requirements, behavior the user or other code relies
  on, user-visible contracts, persisted data.
- **Incidental:** accidents of the current implementation, unused generality
  or configuration, handling of states construction already rules out,
  compatibility with callers that are gone, features that cost more than
  they are used.

Cut incidental behavior by default, with its tests and documentation. Ask
before cutting anything essential or explicitly requested.

## Independent explanation

For each pass, brief a fresh explainer subagent that has no conversation
history and does not read the code. Honor the user's model and effort choices;
if isolated delegation is unavailable, say so.

The brief gives purpose, inputs, outputs, required semantics, examples, and
constraints, and marks each requirement firm or negotiable. It omits current
helper names, structure, and earlier proposals. Ask for the simplest
sufficient model: an explanation, pseudocode, invariants, edge cases, and
which negotiable requirements it would drop and at what cost. Ask for at most
about 1,200 words; open-ended answers about large subsystems can stall.

## Debate

Treat the model as a hypothesis. Working from the top down:

- Ask what breaks if a whole stage, representation, or piece of state goes,
  and what breaks if a negotiable requirement is dropped.
- Prefer derived state to stored state kept in sync, one decision point to
  many, and direct calls to intermediate protocols.
- Trace concrete examples, failures, ordering, and lifetimes.
- Reject generic machinery and moved bookkeeping.

Facts from the code may inform the debate; do not turn incidental
implementation choices into requirements. Share the refined model before
large edits, and ask the user when intended behavior is genuinely unclear.

## Compare and refactor

Ask whether the code directly represents the model, comparing stages and data
representations first, then modules, then functions; a high-level mismatch
usually explains lower ones. Refactor at the model's boundaries with names
from the model. Parse once into structured data instead of interpreting the
same input twice. Add a record, flag, or abstraction only when it clarifies
the model.

Target functions of at most 10 lines after normal formatting. Split at
meaningful operations, never by packing lines or numbering helpers; an
exhaustive dispatch may stay longer, reported as an exception. Follow Per
Vognsen's [Bitwise](https://github.com/tsnl/bitwise) as
[Resin's essay](https://github.com/tsnl/resin/blob/7a19915229414242bad5ecd7763299532610a02b/doc/bitwise.md)
reads it: representations expose the problem's objects, control flow follows
the problem, each helper makes one complete decision, and generic utilities
need a demonstrated use.

## Verify, repeat, report

Make small changes and run the relevant checks. A cut is verified when its
tests are gone and the tests of what remains pass. Keep a short record of
accepted and rejected changes so later passes do not rediscover them; brief
each new explainer fresh. Stop when the model is visible in the code and the
remaining complexity serves real invariants, or when the user's pass limit is
reached. Keeping the code is a valid outcome.

Report the conceptual changes, every cut and why it was incidental, the
verification, and why the loop stopped.
