# General principles (G1-G36) + Environment (E1-E2)

The largest category, because "general code quality" is where most judgment
calls live. The six below come up often enough to spell out in full; the rest
are listed at the end so nothing in the catalog is lost.

## G5: DRY — every piece of knowledge has one authoritative representation

Bad:
```
caTotal = subtotal * 1.0825
nyTotal = subtotal * 1.07
```

Good:
```
TAX_RATES = { CA: 0.0825, NY: 0.07 }
function calculateTotal(subtotal, state)
  return subtotal * (1 + TAX_RATES[state])
```

Duplicated logic isn't just extra typing — it's a second place the next
change has to remember to happen.

## G16: No obscured intent

Don't be clever where clear would do. If the reader has to decode the line
before they can trust it, the line already failed.

Bad: `return (x & 0x0F) << 4 | (y & 0x0F)`
Good: `return packCoordinates(x, y)`

## G23: Prefer polymorphism to a growing if/else chain

Bad:
```
function calculatePay(employee)
  if employee.type == "SALARIED": return employee.salary
  elif employee.type == "HOURLY": return employee.hours * employee.rate
  elif employee.type == "COMMISSIONED": return employee.base + employee.commission
```

Good — each case owns its own behavior, and a new case doesn't touch the
others:
```
SalariedEmployee.calculatePay() -> salary
HourlyEmployee.calculatePay() -> hours * rate
CommissionedEmployee.calculatePay() -> base + commission
```

## G25: Named constants, not magic numbers

Bad: `if elapsedTime > 86400`
Good:
```
SECONDS_PER_DAY = 86400
if elapsedTime > SECONDS_PER_DAY
```

## G30: A function should do one thing

The tell: if you can extract another function from it with a name that
isn't just a restatement, it was doing more than one thing.

## G36: Law of Demeter — avoid train wrecks

Bad: `outputDir = context.options.scratchDir.absolutePath`
Good: `outputDir = context.getScratchDir()`

Reaching through several objects to get a value couples the caller to every
link in that chain, not just the one it actually needs.

## Environment (E1-E2)

Two rules about the project as a whole, not any single piece of code:

- **E1 — One command to build.** Whatever the toolchain — Gradle, Maven, npm,
  pip, cargo — there should be exactly one command a newcomer needs to build
  the project, not a sequence of steps assembled from memory or a wiki page.
- **E2 — One command to test.** Same idea for running the test suite.

## Enforcement checklist

When reviewing code against this file, check for:
- [ ] No duplication (G5)
- [ ] Clear intent, no magic numbers (G16, G25)
- [ ] Polymorphism over a growing conditional chain (G23)
- [ ] Functions do one thing (G30)
- [ ] No Law of Demeter violations (G36)
- [ ] Boundary conditions handled (G3)
- [ ] Dead code removed (G9)

## Full quick reference

| Rule | Principle |
|---|---|
| G1 | Don't mix languages/DSLs in one file without good reason |
| G2 | Implement the behavior the caller actually expects |
| G3 | Handle boundary conditions explicitly |
| G4 | Don't casually override a safety mechanism |
| G5 | DRY — no duplication |
| G6 | Keep a consistent level of abstraction within a unit |
| G7 | Base classes shouldn't know about their subclasses |
| G8 | Minimize what's public; expose only what callers need |
| G9 | Delete dead code |
| G10 | Keep variables declared close to where they're used |
| G11 | Be consistent — same problem, same pattern, everywhere |
| G12 | Remove clutter — unused imports, empty blocks, noise |
| G13 | Don't introduce coupling just to satisfy a tool or convention |
| G14 | Avoid feature envy — a method more interested in another object's data than its own |
| G15 | Avoid selector/mode arguments (see F3) |
| G16 | No obscured intent |
| G17 | Put code where a reader would expect to find it |
| G18 | Prefer instance methods over static when behavior depends on state |
| G19 | Use explanatory intermediate variables for complex expressions |
| G20 | Function names should say exactly what they do |
| G21 | Understand the algorithm before trusting tests alone to validate it |
| G22 | Make dependencies physical/explicit, not implicit |
| G23 | Prefer polymorphism to if/else or switch chains |
| G24 | Follow the language's and project's established style conventions |
| G25 | Named constants, not magic numbers |
| G26 | Be precise — don't leave ambiguity in behavior or naming |
| G27 | Prefer real structure over relying on convention alone |
| G28 | Encapsulate conditionals behind a well-named check |
| G29 | Avoid negative conditionals (`if not disabled`) |
| G30 | Functions should do one thing |
| G31 | Make temporal coupling (order-dependence) explicit in the API |
| G32 | Don't be arbitrary — every structural decision should have a reason |
| G33 | Encapsulate boundary conditions rather than scattering `+1`/`-1` |
| G34 | Keep one level of abstraction per function |
| G35 | Configuration belongs at high levels, not buried deep in logic |
| G36 | Law of Demeter — avoid reaching through multiple objects |
