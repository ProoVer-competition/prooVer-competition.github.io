---
layout: page
title: Example Proofs
permalink: /example-proofs/
nav_order: 3
---

This page presents examples of proofs, both good and evil. An archive containing these examples is available [here](./assets/proover.tgz).

# Correct Proofs

### Example 1
Problem file (`COR001+1.p`):
<pre>
%------------------------------------------------------------------------------
% File     : COR001+1.p : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
fof(a1, axiom, p(a) & ~p(b)).
fof(c, conjecture, ?[X] : ~(p(X) => ![Y] : (p(Y)))).
% SZS output end ListOfFormulae
</pre>

Proof file (`COR001+1.s`):
<pre>
%------------------------------------------------------------------------------
% File     : COR001+1.s : ProoVer 2026
% Proof    : Problems/COR001+1.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(a1, axiom, p(a) & ~p(b), file('Problems/COR001+1.p',a1)).
fof(c, conjecture, ?[X] : ~(p(X) => ![Y] : (p(Y))), file('Problems/COR001+1.p',c)).
fof(s1, negated_conjecture, ![X] : (p(X) => ![Y] : (p(Y))), inference(negated_conjecture, [status(cth)], [c])).
fof(f1, plain, $false, inference(consequence, [status(thm)], [s1, a1])).
% SZS output end Proof

</pre>

### Example 2

Problem file (`COR002+1.p`):
<pre>
%------------------------------------------------------------------------------
% File     : COR002+1.p : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
fof(ax1, axiom, ![Y]: (p(Y) => p(f(Y)))).
fof(ax2, axiom, p(a)).
fof(c, conjecture, p(f(f(a)))).
% SZS output end ListOfFormulae
</pre>

Proof file (`COR002+1.s`):
<pre>
%------------------------------------------------------------------------------
% File     : COR002+1.s : ProoVer 2026
% Proof    : Problems/COR002+1.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(a1, axiom, ![X]: (p(X) => p(f(X))), file('Problems/COR002+1.p',ax1)).
fof(a2, axiom, p(a), file('Problems/COR002+1.p',ax2)).
fof(c, conjecture, p(f(f(a))), file('Problems/COR002+1.p',c)).
fof(s1, negated_conjecture, ~p(f(f(a))), inference(negated_conjecture, [status(cth)], [c])).
fof(s2, plain, p(a) => p(f(a)), inference(instantiate, [status(thm)], [a1])).
fof(s3, plain, p(f(a)) => p(f(f(a))), inference(instantiate, [status(thm)], [a1])).
fof(s4, plain, p(f(f(a))), inference(horn, [status(thm)], [a2, s1, s2, s3])).
fof(s5, plain, $false, inference(consequence, [status(thm)], [s1, s4])).
% SZS output end Proof
</pre>

### Example 3

Problem file (`COR003+1.p`):
<pre>
%------------------------------------------------------------------------------
% File     : COR003+1.p : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
%----At every marriage, there is a bride and groom who are in love
fof(marriage, axiom, 
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
    in_love(Groom, Bride)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y)).
% SZS output end ListOfFormulae
</pre>  

Proof file (`COR003+1.s`):
<pre>
%------------------------------------------------------------------------------
% File     : COR003+1.s : ProoVer 2026
% Proof    : Problems/COR003+1.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
%----At every marriage, there is a bride and groom who are in love
fof(marriage, axiom, 
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
    in_love(Groom, Bride), file('Problems/COR003+1.p',marriage)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y), file('Problems/COR003+1.p',c)).

%----Negate conjecture: nobody is in love
fof(neg_c, negated_conjecture, 
    ~(? [X] :
    ? [Y] :
    in_love(X, Y)), inference(negated_conjecture, [status(cth)], [c])).

%----Skolemize Bride 
fof(bride, plain, 
    ! [Marriage] :
    ? [Groom] :
    in_love(Groom, sK0(Marriage)), inference(skolemize, [status(esa), new_symbols(skolem, [sK0]), skolemize(Bride, sK0(Marriage))], [marriage])).

%----Skolemize Groom
fof(groom, plain, 
    ! [Marriage] :
    in_love(sK1(Marriage), sK0(Marriage)), inference(skolemize, [status(esa), new_symbols(skolem, [sK1]), skolemize(Groom, sK1(Marriage))], [bride])).

%----Instantiate at the known marriage m0
fof(groom_m0, plain, 
    in_love(sK1(m0), sK0(m0)), inference(instantiate, [status(thm)], [groom])).

%----Contradiction
fof(contradiction, plain, 
    $false,
    inference(consequence, [status(thm)], [neg_c, groom_m0])).
% SZS output end Proof
</pre>  

<br/>

# Evil Proofs

### Example 1

Problem file (`EVL001+1.p`):
<pre>
%------------------------------------------------------------------------------
% File     : EVL001+1.p : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
fof(a1, axiom, p(a)).
fof(c, conjecture, ![X] : (p(X))).
% SZS output end ListOfFormulae
</pre>

Proof file (`EVL001+1.s`):
<pre>
%------------------------------------------------------------------------------
% File     : EVL001+1.s : ProoVer 2026
% Proof    : Problems/EVL001+1.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(a1, axiom, p(a), file('Problems/EVL001+1.p',a1)).
fof(c, conjecture, ![X] : (p(X)), file('Problems/EVL001+1.p',c)).
fof(s1, negated_conjecture, ![X] : (~p(X)), inference(negated_conjecture, [status(cth)], [c])).
fof(f1, plain, $false, inference(consequence, [status(thm)], [s1, a1])).
% SZS output end Proof
</pre>

The `negated_conjecture` step is semantically wrong. The correct negation of a universally quantified formula is `? [X] : ~p(X)`.

### Example 2

Problem file (`EVL002+1.p`):
<pre>
%------------------------------------------------------------------------------
% File     : EVL002+1.p : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_PEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
fof(ax1, axiom, ![X] : (f(f(X)) = f(g(X)) | g(f(X)) = f(f(X)))).
fof(c, conjecture, g(f(a)) = f(g(a))).
% SZS output end ListOfFormulae
</pre>

Proof file (`EVL002+1.s`):
<pre>
%------------------------------------------------------------------------------
% File     : EVL002+1.s : ProoVer 2026
% Proof    : Problems/EVL002+1.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_PEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(a1, axiom, ![X] : (f(f(X)) = f(g(X)) | g(f(X)) = f(f(X))), file('Problems/EVL002+1.p',a1)).
fof(c, conjecture, g(f(a)) = f(g(a)), file('Problems/EVL002+1.p',c)).
fof(s1, negated_conjecture, ~(g(f(a)) = f(g(a))), inference(negated_conjecture, [status(cth)], [c])).
fof(s2, plain, f(f(a)) = f(g(a)), inference(deduction, [status(thm)], [a1])).
fof(s3, plain, f(f(a)) = g(f(a)), inference(deduction, [status(thm)], [a1])).
fof(s4, plain, g(f(a)) = f(g(a)), inference(deduction, [status(thm)], [s2, s3])).
fof(s5, plain, $false, inference(deduction, [status(thm)], [s1, s4])).
% SZS output end Proof
</pre>

The `deduction` steps are not correct. From `(f(f(X)) = f(g(X)) ∨ g(f(X)) = f(f(X)))` and `X = a`, we can derive `f(f(a)) = f(g(a))  ∨  g(f(a)) = f(f(a))`, but here, we derive both statement `s1: f(f(a)) = f(g(a))` and `s2: f(f(a)) = g(f(a))` independently. The proof step incorrectly treats a disjunction as if both disjuncts were true. Also, the axiom `a1` in the proof file does not refer to an axiom from the problem file.

### Example 3

Problem file (`EVL003+1.p`):
<pre>
%------------------------------------------------------------------------------
% File     : EVL003+1.p : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
%----At every marriage, there is a bride and groom who are in love
fof(marriage, axiom, 
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
    in_love(Groom, Bride)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y)).
% SZS output end ListOfFormulae
</pre>

Proof file (`EVL003+1.s`):
<pre>    
%------------------------------------------------------------------------------
% File     : EVL003+1.s : ProoVer 2026
% Proof    : Problems/EVL003+1.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
%----At every marriage, there is a bride and groom who are in love
fof(marriage, axiom, 
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
    in_love(Groom, Bride), file('Problems/EVL003+1.p',marriage)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y), file('Problems/EVL003+1.p',c)).

%----Negate conjecture: nobody is in love
fof(neg_c, negated_conjecture, 
    ~(? [X] :
    ? [Y] :
    in_love(X, Y)), inference(negated_conjecture, [status(cth)], [c])).

%----Skolemize Bride 
fof(bride,plain,
    ! [Marriage] :
    ? [Groom] :
      in_love(Groom,sK0(Marriage)),
    inference(skolemize, [status(esa), new_symbols(skolem, [sK0]), skolemize(Bride, sK0(Marriage))], [marriage])).

%----Skolemize Groom     
fof(groom,plain,
    ! [Marriage] :
      in_love(sK0(Marriage),sK0(Marriage)),
    inference(skolemize,[status(esa), new_symbols(skolem, [sK0]), skolemize(Groom, sK0(Marriage))], [bride])).

%----Instantiate at the known marriage m0
fof(groom_m0, plain, 
    in_love(sK0(m0), sK0(m0)), inference(instantiate, [status(thm)], [groom])).

%----Contradiction
fof(contradiction, plain, 
    $false,
    inference(consequence, [status(thm)], [neg_c, groom_m0])).    
% SZS output end Proof
</pre>

This proof is wrong as it reuses the same Skolem symbol `sK0` twice. 

## Example 4

Problem file (`example4_e.p`):
<pre>
%------------------------------------------------------------------------------
% File     : EVL004+1.p : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
%----At every marriage, there is a bride and groom who are in love
fof(marriage, axiom, 
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
    in_love(Groom, Bride)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y)).
% SZS output end ListOfFormulae
</pre>

Proof file (`example4_e_proof.p`):
<pre>
%------------------------------------------------------------------------------
% File     : EVL004+1.s : ProoVer 2026
% Proof    : Problems/EVL004+1.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(marriage, axiom, 
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
    in_love(Groom, Bride), file('Problems/EVL004+1.p',marriage)).


%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y), file('Problems/EVL004+1.p',c)).

%----Negate conjecture: nobody is in love
fof(neg_c, negated_conjecture, 
    ~(? [X] :
    ? [Y] :
    in_love(X, Y)), inference(negated_conjecture, [status(cth)], [c])).

%----Skolemize Bride 
fof(bride,plain,
    ! [Marriage] :
    ? [Groom] :
      in_love(Groom,sK0(Marriage)),
    inference(skolemize, [status(esa), new_symbols(skolem, [sK0]), skolemize(Bride, sK0(Marriage))], [marriage])).

%----Skolemize Groom 
fof(groom,plain,
    ! [Marriage] :
      in_love(Marriage,sK0(Marriage)),
    inference(skolemize, [status(esa), new_symbols(skolem, [sK1]), skolemize(Groom, sK1(Marriage))], [bride])).

%----Instantiate at the known marriage m0
fof(groom_m0, plain, 
    in_love(m0, sK0(m0)), inference(instantiate, [status(thm)], [groom])).

%----Contradiction
fof(contradiction, plain, 
    $false,
    inference(consequence, [status(thm)], [neg_c, groom_m0])).    
% SZS output end Proof
</pre>

The second inference step introduces `sK1` for `Groom`, but the resulting formula is `in_love(Marriage, sK0(Marriage))`. The correct result should be `in_love(sK1(Marriage), sK0(Marriage))`. 