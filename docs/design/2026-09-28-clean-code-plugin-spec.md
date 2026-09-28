# Language-agnostic Clean Code plugin — spec

## Origin

Source material: [ertugrul-dmr/clean-code-skills](https://github.com/ertugrul-dmr/clean-code-skills),
which ports Robert C. Martin's *Clean Code* Chapter 17 rule catalog (66 rules) into
Claude/Antigravity skills, as two separate language tracks: Python and TypeScript. Both
tracks share identical prose for the language-agnostic rules and only differ in code
snippets and one per-language "idiom" category (Python's P1-P3, TypeScript's TS1-TS3).
Both idiom categories are explicitly documented as adaptations of the book's original
Java-specific rules (J1-J3) — so this project is, in effect, un-porting them back toward
the language the rules were written for in the first place, then generalizing.

The source repo requires picking exactly one track per install because skill names
collide across tracks and loading both produces conflicting instructions.

## Goal

One Claude Code plugin, `clean-code`, that carries the same rule catalog without being
tied to a single language, usable while working in Java, Python, TypeScript, Go, or
anything else — including this workspace's Java codebase.

## Packaging

New repo: `github.com/ekadetov/clean-code`, structured as a Claude Code plugin the same
way the user's existing `ccp` plugin (excalidraw) is:

```
clean-code/
├── .claude-plugin/
│   ├── plugin.json        # name: "clean-code"
│   └── marketplace.json   # installable as clean-code@clean-code
├── README.md
└── skills/
    └── clean-code/
        ├── SKILL.md
        └── references/
            ├── names.md      (N1-N7)
            ├── functions.md  (F1-F4)
            ├── comments.md   (C1-C5)
            ├── general.md    (G1-G36, plus E1-E2 as a short subsection)
            ├── tests.md      (T1-T9)
            └── idioms.md     (I1-I3, generalized from J/P/TS)
```

Install path: `/plugin marketplace add ekadetov/clean-code` then
`/plugin install clean-code@clean-code`. The plugin name becomes the skill prefix
automatically — the skill is invoked as `clean-code:clean-code`, no extra config.

One skill, not seven. A single `SKILL.md` routes to whichever `references/*.md` file(s)
apply to the request (progressive disclosure — only the relevant file(s) load into
context). This avoids the trigger ambiguity of seven similarly-named, similarly-described
skills competing to fire, and avoids duplicating the same "when to use" logic seven times.

## Triggering

On-demand only. This plugin does not auto-fire while code is being written or edited —
the user already has several review agents/skills for that (`ecc:code-reviewer`,
`superpowers:code-reviewer`, `pr-review-toolkit`, `gitlab-mr`). It triggers when the user
explicitly asks something the rule catalog answers: renaming, "is this function too big",
"why do we have this comment", "is this DRY", "are these tests thorough enough", "clean
this up", "give this file a clean-code pass".

The source repo's `boy-scout` skill (an always-on orchestrator that nudges "one small
improvement" into every edit) does not carry over as a separate skill under this
triggering model. Its two useful ideas move into the main skill instead:
- Its rule-selection table becomes the router in `SKILL.md`.
- Its "full sweep" behavior becomes an explicit mode: when the user asks for a full
  clean-code pass on a file/PR, the skill reads all six reference files and reports
  violations by rule code, the same way the source repo's master skill does.

## Rule inventory (66 rules, unchanged count from source)

| Category | Codes | Count | File |
|---|---|---|---|
| Comments | C1-C5 | 5 | comments.md |
| Environment | E1-E2 | 2 | general.md (subsection) |
| Functions | F1-F4 | 4 | functions.md |
| General | G1-G36 | 36 | general.md |
| Names | N1-N7 | 7 | names.md |
| Idioms | I1-I3 | 3 | idioms.md |
| Tests | T1-T9 | 9 | tests.md |

Each reference file keeps the source's approach: elaborate the handful of rules that fire
most often with a prose explanation and a before/after (pseudocode, not any one
language), and list the rest as a one-line quick-reference table so nothing from the
catalog is lost even where it isn't spelled out in full.

### Generalizing the idiom category (I1-I3)

The source's P1-P3 / TS1-TS3 are per-language re-expressions of the book's Java rules.
Read back toward the general principle, they are:

- **I1 — Explicit imports.** Avoid wildcard/blanket imports and hidden re-exports; a
  reader should be able to see where a name comes from. (Java: `import package.*;`.
  Python: `from x import *`. TS: barrel-file overuse.)
- **I2 — Closed sets as enums, not raw constants.** When a value is one of a fixed,
  known set, express that with the language's enum/sealed-type mechanism instead of
  loose ints or strings. (Java: `enum`. Python: `enum.Enum`. TS: `enum`/literal unions.)
- **I3 — Explicitly typed boundaries.** Public/exported interfaces should be typed
  explicitly; avoid escape hatches at the boundary. (Java: no raw `Object`/raw generics
  in public APIs. Python: type hints on public interfaces. TS: no `any` at boundaries.)

### Illustrating rules without a baked-in language

Every reference file states principles in prose first. Where a concrete example clarifies
a rule, it uses short, deliberately generic pseudocode — not real Java/Python/TS syntax.
When applying a rule to actual code, illustrate the fix in the language of the code
under discussion, matching that codebase's existing idioms and conventions rather than
defaulting to any one language's syntax.

## SKILL.md shape

Frontmatter:
- `name: clean-code`
- `description`: states what it enforces (Robert C. Martin's Clean Code catalog — names,
  functions, comments, general/DRY principles, tests, language idioms), explicitly says
  it applies to any language (Java, Python, TypeScript, Go, etc., not just one), and
  names concrete trigger phrases (renaming, function-size complaints, "is this
  idiomatic", "is this DRY", "clean-code review", "give this a clean-code pass") so it
  fires on-demand without needing the words "clean code" verbatim.

Body:
1. One-paragraph philosophy (why these rules exist — Clean Code's core thesis).
2. A routing table: request shape → which reference file(s) to read. Mirrors the old
   boy-scout table but points at reference files instead of separate skills.
3. Full-sweep mode instructions: when asked to review a whole file/PR for clean code,
   read all six reference files and report violations by rule code with a suggested
   fix, same reporting convention as the source's master skill (e.g. "G25 violation:
   magic number `86400`, extract to a named constant").
4. A short "language-neutral by default" reminder, so the skill doesn't default to
   Python-flavored examples out of habit.

## Testing plan

Once the plugin is drafted, validate with a small set of realistic prompts covering:
1. A Java naming request (e.g. rename a poorly-named variable in a Spring Boot service).
2. A "is this function doing too much" question against a multi-responsibility method.
3. A "give this file a clean-code pass" full-sweep request, to check the router reads all
   six references and reports by rule code.
4. A should-not-trigger check: a plain "run these tests" request, to confirm the skill
   doesn't fire on-demand triggering that isn't actually about clean-code review.

These will run as skill-creator eval prompts (with-skill vs. no-skill) once the SKILL.md
and references are drafted.

## Deferred / out of scope for v1

- Publishing to GitHub and registering the marketplace — stays local until reviewed.
- A `.skill` package export — can be done any time via skill-creator's packaging step.
- Description-triggering optimization loop — worth running once the rule content is
  stable, not before.
