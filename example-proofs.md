---
layout: page
title: Example Proofs
permalink: /example-proofs/
nav_order: 3
---

# Correct Proofs

### Example 1
Problem file (`example1_c.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example1_c : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
fof(a1, axiom, p(a) & ~p(b)).
fof(c, conjecture, ?[X] : ~(p(X) => ![Y] : (p(Y)))).
% SZS output end ListOfFormulae
</pre>

Proof file (`example1_c_proof.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example1_proof : ProoVer 2026
% Proof    : Problems/example1_c.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(a1, axiom, p(a) & ~p(b), file('Problems/example1_c.p',a1)).
fof(c, conjecture, ?[X] : ~(p(X) => ![Y] : (p(Y))), file('Problems/example1_c.p',c)).
fof(s1, negated_conjecture, ![X] : (p(X) => ![Y] : (p(Y))), inference(negated_conjecture, [status(cth)], [c])).
fof(f1, plain, $false, inference(consequence, [status(thm)], [s1, a1])).
% SZS output end Proof
</pre>

### Example 2

Problem file (`example2_c.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example2_c : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
fof(ax1, axiom, ![X]: (p(X) => p(f(X)))).
fof(ax2, axiom, p(a)).
fof(c, conjecture, p(f(f(a)))).
% SZS output end ListOfFormulae
</pre>

Proof file (`example2_c_proof.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example2_proof : ProoVer 2026
% Proof    : Problems/example2_c.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(a1, axiom, ![X]: (p(X) => p(f(X))), file('Problems/example2_c.p',ax1)).
fof(a2, axiom, p(a), file('Problems/example2_c.p',ax2)).
fof(c, conjecture, p(f(f(a))), file('Problems/example2_c.p',c)).
fof(s1, negated_conjecture, ~p(f(f(a))), inference(negated_conjecture, [status(thm)], [c])).
fof(s2, plain, p(a) => p(f(a)), inference(instantiate, [status(thm)], [a1])).
fof(s3, plain, p(f(a)) => p(f(f(a))), inference(instantiate, [status(thm)], [a1])).
fof(s4, plain, p(f(f(a))), inference(horn, [status(thm)], [a2, s1, s2])).
fof(s5, plain, $false, inference(consequence, [status(thm)], [s1, s4])).
% SZS output end Proof
</pre>

### Example 3

Problem file (`example3_c.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example3_c : ProoVer 2026
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

%----There exists at least one marriage
fof(exists_marriage, axiom, 
    is_marriage(m0)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y)).
% SZS output end ListOfFormulae
</pre>  

Proof file (`example3_c_proof.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example3_proof : ProoVer 2026
% Proof    : Problems/example3_c.p
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
    in_love(Groom, Bride), file('Problems/example3_c.p',marriage)).

%----There exists at least one marriage
fof(exists_marriage, axiom, 
    is_marriage(m0), file('Problems/example3_c.p',exists_marriage)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y), file('Problems/example3_c.p',conjecture)).

%----Negate conjecture: nobody is in love
fof(neg_c, negated_conjecture, 
    ~(? [X] :
    ? [Y] :
    in_love(X, Y)), inference(negated_conjecture, [status(cth)], [c])).

%----Skolemize Bride 
fof(bride, plain, 
    ! [Marriage] :
    ? [Groom] :
    in_love(Groom, sK0(Marriage)), inference(skolemize, [status(esa), new_symbols(skolem, [sK0]), skolemized(Bride), bind(Bride, sK0(Marriage))], [marriage])).

%----Skolemize Groom
fof(groom, plain, 
    ! [Marriage] :
    in_love(sK1(Marriage), sK0(Marriage)), inference(skolemize, [status(esa), new_symbols(skolem, [sK1]), skolemized(Groom), bind(Groom, sK1(Marriage))], [bride])).

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

Problem file (`example1_e.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example1_e : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
fof(a1, axiom, p(a)).
fof(c, conjecture, ![X] : (p(X))).
% SZS output end ListOfFormulae
</pre>

Proof file (`example1_e_proof.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example1_e_proof : ProoVer 2026
% Proof    : Problems/example1_e.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(a1, axiom, p(a), file('Problems/example1_e.p',a1)).
fof(c, conjecture, ![X] : (p(X)), file('Problems/example1_e.p',c)).
fof(s1, negated_conjecture, ![X] : (~p(X)), inference(negated_conjecture, [status(cth)], [c])).
fof(f1, plain, $false, inference(consequence, [status(thm)], [s1, a1])).
% SZS output end Proof
</pre>

The `negated_conjecture` step is semantically wrong. The correct negation of a universally quantified formula is `? [X] : ~p(X)`.

### Example 2

Problem file (`example2_e.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example2_e : ProoVer 2026
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_PEQ
%------------------------------------------------------------------------------
% SZS output start ListOfFormulae
fof(ax1, axiom, ![X] : (f(f(X)) = f(g(X)) | g(f(X)) = f(f(X)))).
fof(c, conjecture, g(f(a)) = f(g(a))).
% SZS output end ListOfFormulae
</pre>

Proof file (`example2_e_proof.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example2_e_proof : ProoVer 2026
% Proof    : Problems/example2_e.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_PEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(a1, axiom, ![X] : (f(f(X)) = f(g(X)) | g(f(X)) = f(f(X))), file('Problems/example2_e.p',a1)).
fof(c, conjecture, g(f(a)) = f(g(a)), file('Problems/example2_e.p',c)).
fof(s1, negated_conjecture, ~(g(f(a)) = f(g(a))), inference(negated_conjecture, [status(thm)], [c])).
fof(s2, plain, f(f(a)) = f(g(a)), inference(deduction, [status(thm)], [a1])).
fof(s3, plain, f(f(a)) = g(f(a)), inference(deduction, [status(thm)], [a1])).
fof(s4, plain, g(f(a)) = f(g(a)), inference(deduction, [status(thm)], [s2, s3])).
fof(s5, plain, $false, inference(deduction, [status(thm)], [s1, s4])).
% SZS output end Proof
</pre>

The `deduction` steps are not correct. From `(f(f(X)) = f(g(X)) ∨ g(f(X)) = f(f(X)))` and `X = a`, we can derive `f(f(a)) = f(g(a))  ∨  g(f(a)) = f(f(a))`, but here, we derive both statement `s1: f(f(a)) = f(g(a))` and `s2: f(f(a)) = g(f(a))` independently. The proof step incorrectly treats a disjunction as if both disjuncts were true. Also, the axiom `a1` in the proof file does not refer to an axiom from the problem file.

### Example 3

Problem file (`example3_e.p`):
<pre>
%------------------------------------------------------------------------------
% File     : example3_e : ProoVer 2026
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

%----There exists at least one marriage
fof(exists_marriage, axiom, 
    is_marriage(m0)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y)).
% SZS output end ListOfFormulae
</pre>

Proof file (`example3_e_proof.p`):
<pre>    
%------------------------------------------------------------------------------
% File     : example3_e_proof : ProoVer 2026
% Proof    : Problems/example3_e.p
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
    in_love(Groom, Bride), file('Problems/example3_e.p',marriage)).

%----There exists at least one marriage
fof(exists_marriage, axiom, 
    is_marriage(m0), file('Problems/example3_e.p',exists_marriage)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y), file('Problems/example3_e.p',conjecture)).

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
    inference(skolemize, [status(esa), new_symbols(skolem, [sK0]), skolemized(Bride), bind(Bride, sK0(Marriage))], [marriage])).

%----Skolemize Groom     
fof(groom,plain,
    ! [Marriage] :
      in_love(sK0(Marriage),sK0(Marriage)),
    inference(skolemize,[status(esa), new_symbols(skolem, [sK0]), skolemized(Groom), bind(Groom, sK0(Marriage))], [bride])).

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
% File     : example4_e : ProoVer 2026
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

%----There exists at least one marriage
fof(exists_marriage, axiom, 
    is_marriage(m0)).

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
% File     : example4_e_proof : ProoVer 2026
% Proof    : Problems/example4_e.p
% Source   : ProoVer 2026
% Status   : Unknown
% SPC      : FOF_UNK_RFO_NEQ
%------------------------------------------------------------------------------
% SZS output start Proof
fof(marriage, axiom, 
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
    in_love(Groom, Bride), file('Problems/example4_e.p',marriage)).

%----There exists at least one marriage
fof(exists_marriage, axiom, 
    is_marriage(m0), file('Problems/example4_e.p',exists_marriage)).

%----Conjecture: someone is in love
fof(c, conjecture, 
    ? [X] :
    ? [Y] :
    in_love(X, Y), file('Problems/example4_e.p',conjecture)).

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
    inference(skolemize, [status(esa), new_symbols(skolem, [sK0]), skolemized(Bride), bind(Bride, sK0(Marriage))], [marriage])).

%----Skolemize Groom 
fof(groom,plain,
    ! [Marriage] :
      in_love(Marriage,sK0(Marriage)),
    inference(skolemize, [status(esa), new_symbols(skolem, [sK1]), skolemized(Groom), bind(Groom, sK1(Marriage))], [bride])).

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