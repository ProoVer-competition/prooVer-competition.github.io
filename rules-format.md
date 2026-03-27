---
layout: page
title: Rules and Format
permalink: /rules-format/
nav_order: 2
---

# Proof Structure

## Language

* Each benchmark consists of one probleme file with axioms and a conjecture (delimited by `% SZS output start ListOfFormulae` and `% SZS output end ListOfFormulae`) and its corresponding proof file (delimited by `% SZS output start Proof` and `% SZS output end Proof`, which also contains axioms and a conjecture).
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


# Rules 

Not all problematic cases are explicitly described in these rules.  
Participants are responsible for handling edge cases and for deciding what should or should not be considered an acceptable proof according to the TSTP standard and general proof-checking principles.


## Specified Rules 

* Skolemization:

  A skolemization inference step must:

  * use the inference rule name `skolemize`;
  * have status `esa`;
  * introduce one new Skolem symbol (for instance, `sK`) using `new_symbols(skolem, [sK])`;
  * explicitly indicate which existential variable (for instance, `Var`) is being skolemized using `skolemized(Var)`;
  * specify the binding between the existential variable and the Skolem term using `bind(Var, sK(...))`.
  
  The Skolem term must depend exactly on the universally quantified variables that are in scope at the point where the existential variable is eliminated.  
  The resulting formula must be a correct Skolemization of the parent formula.  
  See the [TSTP documentation](https://tptp.org/UserDocs/QuickGuide/Derivations.html) and
  [Example 3](./example-proofs.md) in the correct proofs section for detailed illustrations.


## Unspecified Rules

The full list of admissible unspecified inference rules will be provided later.  
For the purpose of this competition, unspecified rules do not need to be hard-coded, as long as they are checked via external ATP calls.