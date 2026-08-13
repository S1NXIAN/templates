# Coding standards — house conventions

# Last updated: 2026-08-13

This is the canonical, language-agnostic coding standards. A repo's own
CODING_STANDARDS.md references this file by link and may add stack-specific
rules. Follow the rules below in any repo that references this file, as
amended by that repo's own standards: when the two disagree, the repo's
document wins — record the divergence in the repo's docs, never edit this
canonical copy. Editing this file happens here, in the templates repo.

## Naming

A good name is the best documentation — the first and most important
documentation a thing has.

- Names reveal intent: a name tells you why a thing exists, what it does,
  and how it is used. A name that needs a comment to explain it is a naming
  failure — rename it, don't annotate it.
- Methods and functions are verbs that say what they do (`fetch_user`,
  `validate_input`); classes, structs, and types are nouns (`UserSession`,
  `CommandRegistry`).
- No vague names: `data`, `stuff`, `result`, `tmp2` do not survive review.
- One concept, one name. The repo's glossary, where the repo keeps one,
  records the ubiquitous language; when code and glossary disagree, one of
  them is wrong — fix one, never both.
- Abbreviations are allowed only when they are the domain's own words
  (SSID, ADR, MCP); never coin private abbreviations.

## Comments

A comment provides information the code itself cannot contain — why the code
is there, which invariant it maintains, which failure mode a guard prevents.
Comments explain why something is done, not what the code is doing.

- The one mandatory comment is a function contract: what it does, its
  signature, return semantics — present tense, the current invariant, never
  the change that produced it.
- Inline comments are for the non-obvious only: why this order matters,
  which guard is load-bearing. If a reader could write the line from the
  comment, the comment is redundant.
- Comments that contradict the code are worse than no comments — a stale
  comment is rewritten or deleted, never left.
- Never comment out code, and never keep changelog-style comments — the
  commit history owns both.

## Modules and seams

- Prefer deep modules: a module's interface should be much simpler than its
  implementation. A shallow module hides little and just adds a call layer.
- A module owns its seam: it is the one place that touches its shared state,
  and callers reach it through that point. A second implementation beside an
  existing one is prohibited — extend the existing module or make the case
  for replacing it.
- Functions that return data print nothing: emit data on stdout, send
  messaging elsewhere, return status.
- One behaviour, one home: changing a behaviour is a one-file edit. If a
  change forces identical edits across many files, the seam is wrong — fix
  the seam, not the callers.
- The environment is a source of truth: one-file, one-command lookups stay
  in config and `--help`; this file records only the unwritten conventions.

## Git

- Conventional Commits, per the spec at conventionalcommits.org: a type, an
  optional scope, a description (`fix(parser): handle empty input`). The
  spec mandates only `fix` and `feat`; the house set is
  `feat`, `fix`, `refactor`, `docs`, `test` — a type outside the set is
  fine when it plainly fits (`perf`, `build`); lower-case types are a house
  convention, not a spec rule.
- One logical change per commit: a single logical change is contained within
  a single commit; when a change spans two types, split it.
- Subject line: imperative mood ("fix", not "fixes" or "fixed"), 50
  characters or fewer, followed by a blank line and detail in the body.
