---
layout: page
title: Example Proofs
permalink: /example-proofs/
nav_order: 3
---

# Correct Proofs

### Example 1

<pre>
fof(a1, axiom, p(a) & ~p(b)).
fof(c, conjecture, ?[X] : ~(p(X) => ![Y] : (p(Y)))).

fof(s1, negated_conjecture, ![X] : (p(X) => ![Y] : (p(Y))), inference(negated_conjecture, [status(cth)], [c])).
fof(f1, plain, $false, inference(consequence, [status(thm)], [s1, a1])).
</pre>

### Example 2

<pre>
fof(a1, axiom, ![X]: (p(X) => p(f(X)))).
fof(a2, axiom, p(a)).
fof(c, conjecture, p(f(f(a)))).

fof(s1, plain, p(a) => p(f(a)), inference(instantiate, [status(thm)], [a1])).
fof(s2, plain, p(f(a)) => p(f(f(a))), inference(instantiate, [status(thm)], [a1])).
fof(s3, plain, p(f(f(a))), inference(horn, [status(thm)], [a2, s1, s2])).
</pre>

### Example 3
<pre>
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
</pre>  

<br/>

# Evil Proofs

### Example 1
<pre>
fof(a1, axiom, p(a)).
fof(c, conjecture, ![X] : (p(X))).
fof(s1, negated_conjecture, ![X] : (~p(X)), inference(negated_conjecture, [status(cth)], [c])).
fof(f1, plain, $false, inference(consequence, [status(thm)], [s1, a1])).
</pre>

The `negated_conjecture` step is semantically wrong. The correct negation of a universally quantified formula is `? [X] : ~p(X)`.

### Example 2
<pre>
fof(a1, axiom, ![X] : (f(f(X)) = f(g(X)) | g(f(X)) = f(f(X)))).

fof(c, conjecture, g(f(a)) = f(g(a))).

fof(s1, plain, f(f(a)) = f(g(a)), inference(deduction, [status(thm)], [a1])).
fof(s2, plain, f(f(a)) = g(f(a)), inference(deduction, [status(thm)], [a1])).
fof(s3, plain, g(f(a)) = f(g(a)), inference(deduction, [status(thm)], [s1, s2])).
</pre>

The `deduction` steps are not correct. From `(f(f(X)) = f(g(X)) ∨ g(f(X)) = f(f(X)))` and `X = a`, we can derive `f(f(a)) = f(g(a))  ∨  g(f(a)) = f(f(a))`, but here, we derive both statement `s1: f(f(a)) = f(g(a))` and `s2: f(f(a)) = g(f(a))` independently. The proof step incorrectly treats a disjunction as if both disjuncts were true.

### Example 3
<pre>
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
</pre>

This proof is wrong as it reuses the same Skolem symbol `sK0` twice. 

## Example 4
<pre>
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
</pre>

The second inference step introduces `sK1` for `Groom`, but the resulting formula is `in_love(Marriage, sK0(Marriage))`. The correct result should be `in_love(sK1(Marriage), sK0(Marriage))`. 