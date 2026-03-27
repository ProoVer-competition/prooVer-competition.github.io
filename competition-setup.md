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

# Computers
The competition computers have:
* An octa-core Intel(R) Xeon(R) E5-2620 v4 @ 2.10GHz, without hyperthreading.
* 128GiB memory
* The CentOS Linux release 7.4.1708 (Core) operating system, Linux kernel 3.10.0-693.el7.x86_64.

One proof checker runs on one CPU at a time. No CPU time limits are imposed (so that it can be advantageous to use all the cores on the CPU).


# Output 
For each input problem, the system must produce exactly one of the following SZS status lines:

* Good proof: `%SZS status Verified`
* Bad proof: `%SZS status FailedVerified`  
* Don't know if it's a good or a bad proof: `%SZS status NotVerified`

Any other output will be treated as a failure for the corresponding problem. You can add additional information for the proof output by using `:`, for instance: 
`%SZS status FailedVerified : Step s2 is incorrect`

# System Evaluation
* 100 proof validation problems:
  * 50 valid proofs
  * 50 invalid proofs
* Time limit: 30 seconds per problem (Wall-clock time)
* Each problem is evaluated independently.

# Grading Scheme
* Identifying a bad proof as bad = +2
* Identifying a good proof as good = +1
* Giving up = 0
* Identifying a good proof as bad = -1
* Identifying a bad proof as good = -10

The final score is the sum over all problems. Ties are broken according to the average time taken over problems solved.  
A trophy will be awarded to the winner. 

# Organization 

For questions, please contact:
* [Julie Cailler](julie.cailler@loria.fr)  
* [Simon Guilloud](simon.guilloud@epfl.ch)