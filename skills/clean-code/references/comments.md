# Comments (C1-C5)

A comment is a promise that will go stale the moment the code around it
changes and nobody remembers to update it. These rules exist to keep the few
comments that survive worth trusting.

## C1: Comments aren't for metadata

Author names, change history, ticket numbers, dates — that's what version
control is for. A comment should carry a technical note about the code, and
nothing else.

## C2: Delete obsolete comments immediately

A comment describing code that no longer exists, or behaves differently now,
is worse than no comment — it actively misleads the next reader. The moment
you notice one, delete or fix it; don't leave it for later.

## C3: Don't restate the code

Bad:
```
i = i + 1   // increment i
user.save() // save the user
```

Good — say the *why*, when the *why* isn't obvious from the code itself:
```
i = i + 1   // compensate for the header row consumed above
```

## C4: If a comment is worth writing, write it well

Choose words carefully, use correct grammar, don't ramble, don't state the
obvious. A sloppy comment costs the same attention as a good one but pays
back less.

## C5: Never commit commented-out code

```
// function oldCalculateTax(income)
//   return income * 0.15
```

Delete it. Nobody can tell how old it is or whether it still matters, and
version control remembers it perfectly well without cluttering the file.

## The goal

The best comment is the one you didn't need to write because the code was
clear enough on its own. If you find yourself reaching for a comment to
explain *what* code does, that's usually a sign to refactor first and comment
(if still needed) after.

## Quick reference

| Rule | Principle |
|---|---|
| C1 | No metadata in comments — that's what Git is for |
| C2 | Obsolete comment → delete it immediately, don't leave it |
| C3 | Don't restate the code; explain the non-obvious why |
| C4 | If it's worth writing, write it well |
| C5 | Never commit commented-out code |
