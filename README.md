# clean-code

A Claude Code plugin that applies Robert C. Martin's *Clean Code* rule catalog
(names, functions, comments, general/DRY principles, tests, and language
idioms — 66 rules total) to any language, not just one.

Adapted from [ertugrul-dmr/clean-code-skills](https://github.com/ertugrul-dmr/clean-code-skills),
which ports the same catalog into separate Python and TypeScript tracks. This
plugin generalizes it into a single language-agnostic skill instead, so it
applies the same way in Java, Python, TypeScript, Go, or anything else.

## Installation

```bash
# Add this repo as a marketplace
/plugin marketplace add ekadetov/clean-code

# Install the plugin
/plugin install clean-code@clean-code
```

## Skill: clean-code

Ask Claude Code things like:

```
"Is this a good name for this variable?"
"This function takes 7 parameters, should I split it?"
"Does this comment still earn its keep?"
"Is this idiomatic Java?"
"Give this file a clean-code pass"
```

The skill routes to whichever rule category applies — naming, functions,
comments, general/DRY, tests, or language idioms — rather than loading the
whole catalog for every question. See `skills/clean-code/SKILL.md` for the
routing table and `skills/clean-code/references/` for the full rule catalog.

## Design notes

See `docs/design/2026-09-28-clean-code-plugin-spec.md` for the reasoning
behind the single-skill structure, the on-demand (not auto-firing) triggering
model, and how the language-idiom rules were generalized.
