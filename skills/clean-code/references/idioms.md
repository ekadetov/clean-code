# Idioms (I1-I3)

Clean Code's original catalog included a small set of Java-specific rules.
Later ports re-expressed them for Python and TypeScript. Read back to the
principle underneath, they generalize to any language with imports, a way
to express a closed set of values, and typed interfaces — which is most of
them:

## I1: Keep imports explicit

A reader should be able to tell where a name comes from without guessing.
Wildcard imports, blanket re-exports, and hidden transitive dependencies all
break that.

- Java: avoid `import package.*;`
- Python: avoid `from x import *`
- TypeScript: avoid barrel-file re-export sprawl that obscures where a symbol
  actually lives

## I2: A fixed, known set of values is an enum, not loose constants

When a value can only ever be one of a small, closed set, say so with the
language's enum (or sealed-type/literal-union) mechanism instead of scattering
raw strings or ints that the compiler/interpreter can't check for you.

Bad: `status = "ACTIVE"` (any string will silently pass)
Good: `status = Status.ACTIVE` (only a real member of the set will)

- Java: `enum`
- Python: `enum.Enum`
- TypeScript: `enum` or a literal union type

## I3: Type public/exported boundaries explicitly

Whatever is visible outside a module — a public method, an exported
function, an API response shape — should say exactly what it accepts and
returns. Escape hatches at the boundary (a raw `Object`, an untyped `dict`,
`any`) push the type-checking work onto every caller instead of doing it
once at the source.

- Java: avoid raw types/`Object` in public signatures; use real generics
- Python: type hints on public interfaces
- TypeScript: no `any` at module boundaries

## Quick reference

| Rule | Principle |
|---|---|
| I1 | Explicit imports — no wildcards, no hidden re-exports |
| I2 | Closed set of values → enum, not raw constants |
| I3 | Type the public boundary explicitly; no untyped escape hatches |
