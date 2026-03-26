---
layout: page
title: Design and Organization
permalink: /competition-setup/
nav_order: 1
---

# System Design & Delivery
* Systems must be fully automatic; i.e., all command lines must be the same for all problems.
* System results must be reproducible by running the system again.
* Entrants must deliver a package containing everything required, including any external tools.

# Output 
For each input problem, the system must produce exactly one of the following SZS status lines:

* Valid proof: `%SZS status Verified`
* Invalid proof: `%SZS status FailedVerified`  
* Gave up: `%SZS status NotVerified`

Any other output will be treated as a failure for the corresponding problem.

# System Evaluation
* 100 proof validation problems:
  * 50 valid proofs
  * 50 invalid proofs
* Time limit: 30 seconds per problem (WC)
* Each problem is evaluated independently.

# Grading Scheme
* Identifying a bad proof as bad = +2
* Identifying a good proof as good = +1
* Giving up = 0
* Identifying a good proof as bad = −1
* Identifying a bad proof as good = −10

The final score is the sum over all problems.

In case of a tie, we will *not* break it.

# Organization 

For questions, please contact:
* Julie Cailler — julie.cailler@loria.fr  
* Simon Guilloud — simon.guilloud@epfl.ch