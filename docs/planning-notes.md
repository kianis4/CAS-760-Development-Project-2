# Project 2 Planning Notes

## Topics we considered

1. Simple networking stack (packets, addresses, routing)
   - Closest to Farmer's IP protocol example
   - But lots of domain-specific concepts to define, might get unwieldy

2. Boolean circuits / logic gates
   - Clean algebraic structure (AND, OR, NOT, composition)
   - Good morphisms (half-adder embeds in full-adder)
   - Might feel too mathematical, similar to Project 1

3. Simple file system (files, directories, permissions)
   - Concrete system everyone understands
   - Natural hierarchy: files -> secured files -> directories
   - Distinct components (permissions vs files) that combine
   - Good development graph structure with shared base theories
   - Easy to explain in presentation

## Chosen topic: Simple File System

We formalize a simple file system with permissions, files, directories,
and file operations. This gives us a clear development graph with
multiple theory definitions, extensions, developments, and a morphism.

Why it works:
- It's a concrete system (not just math), matches the spec
- Has nontrivial complexity: 6-7 modules
- Natural theory graph (not just a linear chain)
- Permissions and files are separate theories that combine into
  secured files -- this gives a real graph shape
- Directory operations give rich development definitions
- Files embed into directories via a morphism (leaf nodes)

## Textbook references

Module format:
- Theory definitions: Def 9.1, p.99
- Theory extensions: Def 9.12, p.105
- Development definitions: Def 9.19, p.108
- Theory translations: Def 14.19, p.192
- Morphism Theorem: Thm 14.16, p.192

Inductive types (relevant for directory tree structure):
- Definition 10.1, p.133
- Example 10.7, p.136 (labelled trees -- similar to directory trees)

Monoids paper structure:
- Follow Defs 4.1-5.1 of Farmer & Zvigelsky 2025 as template
  for how to organize modules

## Theory graph

```
  PERM                FILE
  (permissions)       (basic files)
       \               /
     SECURED-FILE ----
          |
      DIRECTORY
       /       \
   DIR-DEV    FILE-to-DIR
              (morphism)
```

PERM and FILE are independent base theories.
SECURED-FILE extends both (adds permissions to files).
DIRECTORY extends SECURED-FILE with tree structure.
DIR-DEV develops directory operations.
FILE-to-DIR morphism shows files embed as leaf directories.

## Module breakdown

### PERM (theory definition)
- Base type: P (permission level)
- Constants: none, read, write, admin (permission values)
- Axioms: disjointness (none != read, read != write, etc.),
  ordering (read < write < admin), structural induction
- Models a simple permission hierarchy

### FILE (theory definition)
- Base types: N (name), C (content), F (file)
- Constants: mkfile_{N->C->F} (constructor), fname_{F->N}, 
  fcontent_{F->C}
- Axioms: TOTAL(mkfile), INJ(mkfile),
  accessor axioms (fname(mkfile(n,c)) = n, etc.)

### SECURED-FILE (theory extension of FILE)
- Extends FILE
- Adds permission association
- New constants: perm_{F->P} (get permission of file),
  protect_{F->P->F} (set permission)
- Axioms: protect preserves content, perm(protect(f,p)) = p

### DIRECTORY (theory extension of SECURED-FILE)
- Extends SECURED-FILE
- New base type: D (directory)
- Constants: emptydir_D, addentry_{F->D->D}, entries_{D->{F}}
- Axioms: inductive structure (emptydir, addentry as constructors),
  disjointness, structural induction
- Parallels list structure (emptydir <-> nil, addentry <-> cons)

### DIR-DEV (development of DIRECTORY)
- Def: contains_{N->D->o} (check if name exists in directory)
- Thm: TOTAL(contains)
- Def: lookup_{N->D->F} (find file by name)
- Validation: prove lookup RHS is defined
- Def: size_{D->N} (number of entries) -- where N is natural number type
- Thm: TOTAL(size)
- Thm: size(emptydir) = 0
- Thm: size(addentry(f,d)) = size(d) + 1
  (assuming name not already in d)

### FILE-to-DIR (theory morphism)
- Source: FILE, Target: DIRECTORY
- Maps F -> D (a file viewed as a single-entry directory)
- By Morphism Theorem (14.16), FILE theorems transport to DIRECTORY

## Lessons from Project 1 (80/100)

Key feedback from Dr. Farmer to address this time:

1. Derived operations in developments, NOT as constants (-16 pts on P1)
   Farmer: "define <.> in a development of GRP"
   --> contains, lookup, size go in DIR-DEV as Defs.
       Only constructors (mkfile, emptydir, addentry) are ext constants.

2. Define predicates before using them in theorems (-2 pts)
   Farmer: "where is this defined?" (on GROUP predicate)
   --> If we need a predicate like IS-VALID-DIR, define it first.

3. Validate every Def (prove RHS is defined) (-4 pts)
   Farmer: "you also need to validate Def1"
   --> Every Def in DIR-DEV gets a validation proof in the appendix.

4. Formula precision (-2 pts each on Thm2, Thm3)
   --> Triple-check quantifier scopes, type annotations, connectives.

5. Prove TOTAL for every function introduced.

## Work division (to discuss with Hamza)

Options:
- Suleyman: FILE, SECURED-FILE, DIRECTORY, DIR-DEV
- Hamza: PERM, FILE-to-DIR morphism, examples, presentation lead
- Or split differently based on preference
- Each person presents their own modules

## Presentation plan (15 min + 5 min Q&A)

1. Motivation: why a file system? connection to the spec (~2 min)
   - Real-world system, nontrivial complexity, natural graph structure
2. Theory graph overview (~1 min)
3. Walk through modules: PERM, FILE, SECURED-FILE, DIRECTORY (~6 min)
4. DIR-DEV: key definitions and theorems (~3 min)
5. Morphism: FILE-to-DIR, what Thm 14.16 gives us (~2 min)
6. Conclusion (~1 min)

Why this covers the scope:
- 6 modules forming a genuine graph (not linear chain)
- Multiple theory definitions and extensions
- Development definition with Defs + Thms + validations
- Theory morphism
- Follows monoids paper structure
- Incorporates all feedback from Project 1
