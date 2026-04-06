# CAS 760 -- Development 2 Project

## OCaml Core Types in Alonzo

**Suleyman Kiani & Hamza**
McMaster University, Winter 2026
Instructor: Dr. William M. Farmer

---

### Overview

We formalize OCaml's core algebraic data types -- booleans, option types,
lists, and binary trees -- as inductive types in Alonzo, using the little
theories method. Each type is specified as a theory with constructors,
disjointness/injectivity axioms, and a structural induction principle
(Chapter 10 of the textbook). Derived operations (length, append, etc.)
are defined in development definitions. A theory morphism from PA to
lists shows that lists generalize natural numbers.

### Theory Graph

```
BOOL-TYPE              OPTION-TYPE
(enumeration)          (sum type)

PA ----morphism----> LIST-TYPE          TREE-TYPE
                        |                   |
                        v                   v
                     LIST-DEV           TREE-DEV
```

### Building

```bash
pdflatex main.tex && pdflatex main.tex   # two passes for TOC
pdflatex presentation.tex
```

### Files

| File | Description |
|------|-------------|
| `main.tex` | Main paper (Alonzo modules + proofs) |
| `presentation.tex` | Beamer slides |
| `alonzo-notation.tex` | Alonzo LaTeX macros (course-provided) |
| `simple-type-theory-def.tex` | STT definitions (course-provided) |
| `docs/planning-notes.md` | Project planning, references, feedback notes |

### References

1. W.M. Farmer, *Simple Type Theory*, 2nd ed., Birkhauser, 2025.
2. W.M. Farmer & D.Y. Zvigelsky, "Monoid Theory in Alonzo," *Journal of Applied Logics*, vol. 12, no. 7, 2025.
