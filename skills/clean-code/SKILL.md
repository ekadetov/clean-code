---
name: clean-code
description: >
  Applies Robert C. Martin's Clean Code rule catalog (naming, function design,
  comments, DRY/general principles, tests, and language idioms) to any
  language — Java, Python, TypeScript, Go, Kotlin, whatever the current file
  is written in. Use this whenever the user asks to rename or judge a
  variable/function/class name, complains a function or method is doing too
  much or takes too many parameters, questions whether a comment earns its
  keep, asks if something violates DRY or single-responsibility, wants a
  magic number or a long if/else chain cleaned up, asks whether tests cover
  enough edge cases or run too slow, asks something is "idiomatic" for a
  language, or explicitly asks for a "clean code" review or a "clean-code
  pass" over a file/PR. Trigger even when the user doesn't say "clean code" —
  "is this name any good", "split this function", "why is this comment here",
  "are we missing edge cases", "is this the idiomatic way to do X in <lang>"
  all qualify.
---

# Clean Code

Robert C. Martin's *Clean Code* catalogs the small, boring-looking choices that
separate code a team can keep moving in from code that quietly calcifies. None
of it is language-specific — it's about what a name promises, how much a
function tries to do, whether a comment earns the attention it costs, whether
a piece of knowledge lives in one place or five. Apply it the same way whether
the file in front of you is Java, Python, TypeScript, or anything else — only
the syntax of the fix changes, never the principle.

## Routing

Read the reference file(s) that match what's actually being asked. Don't load
all of them for a narrow question — that defeats the point of splitting them
up.

| The user is asking about... | Read |
|---|---|
| A variable/function/class/module name, "what should I call this", "is this ambiguous" | `references/names.md` |
| Function size, parameter count, flag arguments, functions that mutate their inputs, dead functions | `references/functions.md` |
| Whether a comment/docstring is worth keeping, stale comments, commented-out code | `references/comments.md` |
| DRY, single responsibility, magic numbers, long if/elif chains, deep property chains, one-command build/test | `references/general.md` |
| Test coverage, boundary cases, flaky/slow/skipped tests | `references/tests.md` |
| "Is this idiomatic", imports, magic constants vs. enums, typed public interfaces | `references/idioms.md` |
| A full clean-code pass over a file, diff, or PR | all six — see Full-sweep mode below |

If a request spans categories (e.g. "review this function" touches both
`names.md` and `functions.md`), read all the ones that apply — just not the
ones that don't.

## Full-sweep mode

When asked to give a whole file, diff, or PR a clean-code pass ("clean this
up", "does this hold up to clean code", "anything else obviously wrong here"),
read all six reference files, then report findings the same way the rule
catalog is organized: by rule code, one line each, with the fix.

```
G25 violation: magic number 86400 in isExpired() — extract to a named constant
F1 violation: createUser() takes 7 arguments — group related ones into a type
N6 violation: strName, lstUsers — drop the type-encoding prefix
```

Keep the fix proportional to what's actually there — this is Martin's Boy
Scout Rule ("always leave the code a little better than you found it"), not a
mandate to rewrite everything in scope. Note what you fixed inline as you go;
don't produce a separate report nobody asked for unless the request was
specifically for a review.

## Stay language-neutral

The reference files illustrate rules with short, deliberately generic
pseudocode, not real syntax from any one language — because the same rule
applies whether the fix is a Java `enum`, a Python `Enum`, or a TypeScript
union type. When you actually apply a rule to real code, write the fix in
that code's own language and match its existing conventions. Don't reach for
Python-flavored examples out of habit just because the source material for
this catalog happened to use them.

## Quick reference (most common)

The full catalog is 66 rules across the six files above. These are the ones
that come up often enough to keep at hand without opening a reference file:

| Rule | Principle |
|---|---|
| N1 | Names should reveal intent |
| N6 | Don't encode type/scope into a name (`strName`, `IUserRepo`) |
| F1 | More than ~3 parameters means the function does too much or needs a type |
| F3 | A boolean flag parameter means the function does two different things |
| C1 | Comments aren't for metadata — that's what Git is for |
| C3 | Don't restate the code; explain the *why* only when it's non-obvious |
| G5 | DRY — every piece of knowledge has one authoritative representation |
| G25 | Magic numbers become named constants |
| G30 | A function should do one thing |
| G36 | Law of Demeter — avoid reaching through multiple objects (`a.b.c.d`) |
| T5 | Test boundary conditions, not just the happy path |
| T9 | Slow tests don't get run — keep them fast |
| I2 | A fixed, known set of values is an enum, not loose constants |
