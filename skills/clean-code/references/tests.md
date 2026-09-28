# Tests (T1-T9)

Tests are the safety net that lets the rest of this catalog get applied
without fear — rename something, split a function, remove duplication, and
the tests tell you immediately if you broke behavior. That only works if the
tests themselves are trustworthy.

## T1: Test everything that could possibly break

Coverage tools are a guide to gaps, not a target to hit for its own sake.

Bad — only the happy path:
```
test "divide" -> assert divide(10, 2) == 5
```

Good — the cases that could actually break it:
```
test "divide normal"   -> assert divide(10, 2) == 5
test "divide by zero"  -> assert divide(10, 0) raises
test "divide negative" -> assert divide(-10, 2) == -5
```

## T2: Use a coverage tool, and act on what it shows

A coverage report that nobody looks at isn't testing infrastructure, it's
decoration.

## T3: Don't skip trivial tests

A test that documents "new users default to role X" is worth more than its
one line costs — it catches the regression where someone quietly changes
that default.

## T4: A skipped test is an unanswered question, not a solved one

Bad — hides a problem:
```
skip test "async operation" reason: "flaky, fix later"
```

Good — either fix it, or document precisely why it's skipped:
```
skip test "cache invalidation" reason: "requires Redis, see setup docs"
```

## T5: Test boundary conditions explicitly

Bugs cluster at edges — first item, last item, empty input, one-past-the-end,
zero, negative. Test them on purpose rather than hoping the happy-path tests
happen to cover them.

```
test "pagination: first page"
test "pagination: last page"
test "pagination: page past the end -> empty"
test "pagination: page zero -> invalid"
test "pagination: empty list"
```

## T6: When you find a bug, exhaustively test its neighbors

Bugs rarely live alone. An off-by-one in date math is a signal to check every
date boundary, not just the one that broke.

## T7: A pattern in test failures usually points past the tests

If every test in one category fails intermittently, the tests probably
aren't the problem — something structural underneath is (shared state, a
race condition, an external dependency).

## T8: Coverage gaps often reveal design problems

If a function is hard to test in isolation, that's usually because it's
doing too much or reaching too far outside itself (see F1, G30, G36) — the
fix is often to the design, not the test.

## T9: Tests must be fast

Slow tests stop getting run, and a test suite nobody runs stops catching
anything.

Bad — hits a real dependency:
```
test "user creation"
  db = connectToRealDatabase()
  ...
```

Good — isolated and fast:
```
test "user creation"
  db = InMemoryDatabase()
  ...
```

## Test organization

**F.I.R.S.T.** — Fast, Independent, Repeatable, Self-validating, Timely
(written before or alongside the code, not as an afterthought).

**One concept per test.** A test asserting five unrelated things about an
object means five failures collapse into one unhelpful red line. Split it so
each test's name says exactly what broke.

## Quick reference

| Rule | Principle |
|---|---|
| T1 | Test everything that could break, not just the happy path |
| T2 | Use a coverage tool and act on it |
| T3 | Don't skip trivial tests — they document behavior |
| T4 | A skipped test needs a real reason, or it needs fixing |
| T5 | Test boundary conditions on purpose |
| T6 | Found a bug → test its neighbors exhaustively |
| T7 | A failure pattern usually points past the test itself |
| T8 | Hard-to-test code often means a design problem |
| T9 | Tests must be fast, or they stop getting run |
