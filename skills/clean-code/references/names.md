# Names (N1-N7)

Names are the cheapest form of documentation and the easiest to get wrong.
A name is a promise to the reader about what a thing is or does — every rule
here is really the same rule: don't break that promise.

## N1: Names should reveal intent

If a name needs a comment to explain it, the name failed, not the comment.

Bad:
```
d = 86400
```
```
function proc(list)
  return filter(list, item -> item > 0)
```

Good:
```
SECONDS_PER_DAY = 86400
```
```
function filterPositiveNumbers(numbers)
  return filter(numbers, n -> n > 0)
```

## N2: Match the name's level of abstraction to the thing it names

Don't leak implementation details into a name; name what the thing *is*, not
how it's currently built.

Bad: `getDictOfUserIdsToNames()` — bad
Good: `getUserDirectory()` — the caller shouldn't care it's a dict today

## N3: Use standard nomenclature where one exists

Reach for the domain's own vocabulary, or a well-known pattern name, before
inventing new terms. `UserFactory.create(...)`, `calculateAmortization(...)` —
a reader who knows the domain or the pattern doesn't need an explanation.

## N4: Names should be unambiguous

`rename(old, new)` doesn't say what's being renamed — a file? a key? a
variable? `renameFile(oldPath, newPath)` does.

## N5: Match name length to scope size

A loop index living for three lines can be `i`. A module-level constant
living for the life of the program deserves a real name — `MAX = 5` at that
scope is actively misleading; `MAX_RETRY_ATTEMPTS_BEFORE_FAILURE = 5` earns
its length by how long it's visible for.

## N6: Don't encode type or scope into the name

Modern editors already know the type. Hungarian notation (`strName`,
`lstUsers`, `iCount`) and interface prefixes (`IUserRepository`) are relics —
drop them: `name`, `users`, `count`, `UserRepository`.

## N7: A name should describe every side effect

If a function does more than its name promises, the name is a lie, not just
an omission.

Bad:
```
function getConfig()
  if not exists(configPath)
    write(configPath, "{}")   // hidden side effect
  return read(configPath)
```

Good — the name now tells the truth:
```
function getOrCreateConfig()
  if not exists(configPath)
    write(configPath, "{}")
  return read(configPath)
```

## Quick reference

| Rule | Principle |
|---|---|
| N1 | Descriptive names — if it needs a comment, rename it |
| N2 | Name the abstraction, not the implementation |
| N3 | Use the domain's or pattern's standard term |
| N4 | Unambiguous — a reader shouldn't have to guess |
| N5 | Longer scope, longer name; tiny scope, short name is fine |
| N6 | No type/scope encoding (`strName`, `IUserRepo`) |
| N7 | A name must describe every side effect it has |
