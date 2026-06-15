---
layout: page
title: Rules and Format
permalink: /rules-format/
nav_order: 2
---

# Proof Structure

## Language

* Each benchmark consists of one problem file with axioms and a conjecture and its corresponding proof file. Note that, in order to connect both files, there is a field `Proof: path/to/problem_file.p` in the proof file's header. All problem files will be located into a `Problems/` folder. See the [example](./example-proofs.md) page for more information. 
* Proofs are written in TSTP format (see the TSTP Quick Guide: https://tptp.org/UserDocs/QuickGuide/Derivations.html).
* Problems are FOF (First-Order Form) problems.
* Each problem contains a set of axioms and one conjecture.

## Proof Format 
* A proof is a sequence of inference steps.
* All proofs are by refutation: the conjecture is negated, and the proof ends with `$false`.
* Each proof step has:
  * a role: `axiom`, `conjecture`, `negated_conjecture`, or `plain`
  * a status: `thm`, `esa`, or `cth`
* Proofs are expected to have reasonable granularity (i.e., they should be checkable by a standard ATP system).
* The inference rules specified below must be verified internally by the proof checker (e.g., Skolemization, introduction of the negated conjecture).
* Unspecified inference steps may be validated by invoking an external ATP, but only for steps marked with status `thm` or `cth`.
* The checker may only invoke an ATP on a given step using that step and its premises.
* It is forbidden to re-check the entire conjecture independently as a separate sanity check.
* There are no restrictions on the order of proof steps.
* All provided proofs will be syntactically well-formed and parsable in TPTP.
* Axioms and the conjecture must be imported from the corresponding problem file, for instance : `fof(a1, axiom, ![X]: p(X), file('Problems/example_c.p', a1)).`. We guarantee that, in correct and incorrect proofs, the file given in the `file` directive will correspond to the one given in the header of the proof file. This can be trusted and, consequently, the verifier never needs to parse the header.


# Rules 

Not all problematic cases are explicitly described in these rules.  
Participants are responsible for handling edge cases and for deciding what should or should not be considered an acceptable proof according to the TSTP standard and general proof-checking principles.


## Specified Rules 

* Skolemization:
  A skolemization inference step must:

  * use the inference rule name `skolemize`;
  * have status `esa`;
  * introduce one new Skolem symbol (for instance, `sK`) using `new_symbols(skolem, [sK])`;
  * explicitly indicate which existential variable (for instance, `Var`) is being skolemized, together with the the resulting Skolem term, using `skolemize(Var, sk(...))`.
  
  The Skolem term must depend exactly on the universally quantified variables that are in scope at the point where the existential variable is eliminated.  
  The resulting formula must be a correct Skolemization of the parent formula.  
  See the [TSTP documentation](https://tptp.org/UserDocs/QuickGuide/Derivations.html) and
  [Example 3](./example-proofs.md) in the correct proofs section for detailed illustrations.  

* Negated conjecture:
  A negated conjecture step must:
  * use the inference rule name `negated_conjecture`;
  * have the status `cth`;
  * have a parent which is a `conjecture`;
  * have a formula that is the negation of the parent. This last point can be checked either internally, or using an external ATP.   


* Axioms and conjecture:
  An axiom step must:
  * have role `axiom`;
  * have status `thm`;
  * include a `file` directive referencing the corresponding problem file (guaranteed to be the same as the one in the header) and the name of
    the axiom in that file, for instance:
    `fof(ax1, axiom, ![Y]: p(Y), file('Problems/example_c.p', a1)).`;
  * have a formula that is alpha-equivalent to the formula named in the `file` directive
    within the referenced problem file. This point can be checked either internally, or using an external ATP. 

  The name of the axiom step in the proof file may differ from the name used in the
  problem file. The checker must verify that the referenced axiom exists in the problem
  file and that the two formulas are equal up to alpha-equivalence.


## Unspecified Rules

The full list of admissible unspecified inference rules will be provided later.  
For the purpose of this competition, unspecified rules do not need to be hard-coded, as long as they are checked via external ATP calls.