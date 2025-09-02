---
layout: page
title: Rules and Format
permalink: /rules-format/
nav_order: 2
---

# Proof Structure

## Language
* Each problem consists of one problem file and its corresponding solution file.
* Proofs are written in TSTP format: [TSTP Quick Guide](https://tptp.org/UserDocs/QuickGuide/Derivations.html).
* Proofs comes from FOF problems, and contains axioms and a conjecture.

## Proof Format 
* A proof is a sequence of inference steps.
* All proofs are by refutation: the conjecture is negated, and the proof ends at `$false`.
* Each proof step has a role: `axiom`, `conjecture`, `negated_conjecture`, or `plain`.
* Each proof step has a status: `thm`, `esa`, or `cth`.
* Proofs should have ``reasonable'' granularity.
* Specified inference rules (see below) must be checked internally by the proof checker (e.g., Skolemization, negated conjecture, etc.).
* Unspecified inference rules can be checked by an external tool (`thm` only).
* There are no restrictions on the order of the proof steps.

# Rules 

## Specified Rules 
* Skolemization
* Tseitin
* [Add any additional specified rules here]

## Unspecified Rules
* [List of unspecified rules or leave for future addition]
