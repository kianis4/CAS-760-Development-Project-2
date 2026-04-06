# Project 2 Planning Notes

## Topic selection: OCaml Core Types in Alonzo

Why this topic works for the project:

- OCaml's algebraic data types (bool, option, list, tree) map directly to
  inductive types in Chapter 10 of the textbook
- Gives us nontrivial complexity: 4 theory definitions + 2 development
  definitions + 1 theory morphism = 7 modules total
- The project spec says the system should have "a nontrivial level of
  complexity" and gives IP protocol as an example. Our hierarchy of
  data types with developments and a morphism should meet that bar.
- The list type mirrors PA structurally (nil <-> 0, cons <-> S), which
  gives us a natural theory morphism
- We can follow the monoids paper as a structural template

## Textbook references

Inductive types (main reference for this project):
- Definition 10.1, p.133 -- what an inductive type is
- Definition 10.3, p.134 -- axioms for inductive types
- Example 10.5, p.135 -- booleans as enumeration inductive type
- Example 10.7, p.136 -- labelled ordered binary trees

PA (needed for the morphism):
- Theory Def 9.7, p.103

Theory morphisms:
- Morphism Theorem: Thm 14.16, p.192
- Theory translations: Def 14.19, p.192

Module format to follow:
- Monoids paper (Farmer & Zvigelsky 2025), Defs 4.1--5.1

## Planned theory graph

```
BOOL-TYPE (standalone)       OPTION-TYPE (standalone)

PA ---morphism---> LIST-TYPE            TREE-TYPE
                      |                     |
                   LIST-DEV             TREE-DEV
```

- BOOL-TYPE and OPTION-TYPE are independent theory definitions
- LIST-TYPE and TREE-TYPE are recursive inductive types
- LIST-DEV and TREE-DEV are development definitions where we define
  derived operations (length, append, reverse, size, height, mirror)
- Theory morphism from PA to LIST-TYPE maps N -> L, 0 -> nil, S -> cons(*)
- By the Morphism Theorem (14.16), all PA theorems transport to lists

## Module breakdown

BOOL-TYPE
- Base type: B
- Constants: true, false
- Axioms: disjointness (true != false) + structural induction
- Simplest module, good starting point

OPTION-TYPE
- Base types: A (element), O (option)
- Constants: none, some
- Axioms: TOTAL(some), disjointness, INJ(some), structural induction

LIST-TYPE
- Base types: E (element), L (list)
- Constants: nil, cons
- Axioms: TOTAL2(cons), disjointness, INJ(cons), structural induction

LIST-DEV (development of LIST-TYPE)
- Define: length, append, reverse
- Prove: TOTAL for each, associativity of append, identity laws,
  reverse involution
- Remember to validate each Def (prove RHS is defined)

TREE-TYPE
- Base types: V (value), T (tree)
- Constants: leaf, node
- Axioms: TOTAL(leaf), TOTAL2(node), disjointness, INJ(leaf),
  INJ(node), structural induction

TREE-DEV (development of TREE-TYPE)
- Define: size, height, mirror
- Prove: TOTAL for each, mirror involution

NAT-to-LIST
- Theory morphism from PA to LIST-TYPE (with E = unit type)
- N -> L, 0 -> nil, S -> cons(*)

## Lessons from Project 1 (80/100)

Key feedback from Dr. Farmer that we need to address:

1. Derived operations go in developments, NOT as theory extension
   constants. We lost 16 pts because we introduced the generator
   function as a constant instead of defining it in a development.
   Farmer wrote: "define <.> in a development of GRP."
   --> For this project: length, append, reverse, size, height,
       mirror are all Defs in developments. Only constructors
       (nil, cons, leaf, node, none, some) are theory ext constants.

2. Every predicate used in a theorem must be formally defined first.
   We lost 2 pts because GROUP(...) was used in Thm8 but never
   defined. Farmer: "where is this defined?"
   --> If we use any predicate in a theorem, make sure it has a
       prior Def in the development.

3. Every Def must be validated -- prove the RHS is defined. (-4 pts)
   Farmer: "you also need to validate Def1 (i.e., prove RHS is defined)"
   --> Include a validation proof for each Def in the appendix.

4. Formula precision matters. Quantifier scope errors on Thm2 and
   Thm3 cost us 2 pts each.
   --> Triple-check every formula before submission.

5. Farmer also suggested we could define a finite cyclic group and
   state that each member has the form g^n. Good note for richer
   developments -- we should aim for similarly rich content in our
   developments here.

## Work division (TBD with Hamza)

Options:
- Split by data type: one does list modules, other does tree modules,
  share bool/option
- Split by deliverable: one focuses on paper, other on presentation
- Either way, each person should be able to present their own work

## Presentation outline (15 min + 5 min Q&A)

Rough plan:
1. Motivation: why OCaml types, connection to inductive types (~2 min)
2. Theory graph overview (~1 min)
3. Walk through each module: show theory defs, key defs/thms (~8 min)
4. Theory morphism: PA -> lists, what Thm 14.16 gives us (~2 min)
5. Conclusion + questions (~2 min)

This covers the scope because we have:
- Multiple theory definitions (4 types)
- Development definitions with Defs + Thms (2 developments)
- A theory morphism
- Connection between PL theory and formal logic
- Following the monoids paper structure
