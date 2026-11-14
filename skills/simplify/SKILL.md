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
exhaustive dispatch may stay longer, reported as an exception.

## Style

Code communicates a model of the problem: a reader should find the important
objects, follow what happens to them, and predict where a change belongs. The
reference is Per Vognsen's [Bitwise](https://github.com/tsnl/bitwise), whose
Ion compiler explains its machinery while making it work.

- **The representation explains the operations.** A data definition says what
  a value is; its constructor follows from its shape. A reader should
  understand a node from its definition alone, without consulting a scope, a
  callback, or the code that will later visit it. A constructor earns its
  place by establishing an invariant or naming a meaningful conversion.
- **Control flow follows the problem.** Calls, loops, and recursion should
  mirror the subject: a parser's call graph shows precedence, and the loop
  that accumulates a left operand shows left associativity. Repetition that
  keeps the structure easy to survey is fine; share code only when that makes
  the structure clearer, not merely shorter.
- **A helper owns a complete decision.** Obtaining a canonical, interned
  object is one operation, lookup, creation and registration together, rather
  than steps each caller must coordinate. Reuse means answering the same
  question for every caller; similar-looking code may implement different
  rules. A name promises exactly what the helper establishes, with no hidden
  second contract.
- **Small functions hold one coherent thought.** The 10-line target prompts a
  look at a boundary; it is not the goal. One longer loop that attaches every
  postfix form can read better than a chain of vague forwarding helpers. An
  exhaustive dispatch is an inventory of the cases an operation handles; keep
  it together, extracting a case only when it has a name and reasoning of its
  own.
- **Existing vocabulary first.** Use the language's collections, ownership and
  structures; a custom utility needs a concrete need they express poorly.
  Shared utilities serve recurring needs, while the problem's rules stay in
  the code that uses them.
- **Comments explain what the code cannot show:** invariants, ownership,
  lifetimes, and deliberate compromises, kept beside the representation they
  describe. Assertions make assumptions executable. A comment that narrates
  long bookkeeping suggests a missing operation or a representation that
  needs too much coordination.
- **State has an owner.** Each pass takes a bounded input and produces a
  bounded output. A value that certifies data should travel with that data
  and be lost when the data is edited, so a stale "checked" flag cannot
  survive. Prefer immutable inputs to tables that must be kept in sync.

When a bug appears, trace the operation and the facts it needs. Two paths
making the same decision call for one shared operation; lost state calls for
representing it at the boundary that needs it. A new callback or parallel
bookkeeping map helps only if it makes the relationship clearer and has an
owner. A successful cleanup leaves the next reader fewer facts to coordinate.

## Verify, repeat, report

Make small changes and run the relevant checks. A cut is verified when its
tests are gone and the tests of what remains pass. Keep a short record of
accepted and rejected changes so later passes do not rediscover them; brief
each new explainer fresh. Stop when the model is visible in the code and the
remaining complexity serves real invariants, or when the user's pass limit is
reached. Keeping the code is a valid outcome.

Report the conceptual changes, every cut and why it was incidental, the
verification, and why the loop stopped.
