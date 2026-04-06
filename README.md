# CAS 760 -- Development 2 Project

## Simple File System in Alonzo

**Suleyman Kiani & Hamza**
McMaster University, Winter 2026
Instructor: Dr. William M. Farmer

---

### Overview

We formalize a simple file system as a development graph in Alonzo,
using the little theories method. The system models files, permissions,
secured files, and directories as a hierarchy of theories connected
by extensions and morphisms. Derived operations (find, path lookup,
permission checks) are defined in development modules.

### Theory Graph

```
  PERM                FILE
  (permissions)       (basic files)
      \                /
    SECURED-FILE -----
         |
     DIRECTORY
      /       \
  DIR-DEV    FILE-to-DIR
             (morphism)
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
| `docs/planning-notes.md` | Planning, references, topic selection, feedback notes |

### References

1. W.M. Farmer, *Simple Type Theory*, 2nd ed., Birkhauser, 2025.
2. W.M. Farmer & D.Y. Zvigelsky, "Monoid Theory in Alonzo," *Journal of Applied Logics*, vol. 12, no. 7, 2025.
