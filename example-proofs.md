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
%----Starting formula
fof(marriage,plain,
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
      in_love(Groom,Bride) ).

%----Skolemize Bride
fof(bride,plain,
    ! [Marriage] :
    ? [Groom] :
      in_love(Groom,sK0(Marriage)),
    inference(skolemize,[status(esa),new_symbols(skolem,[sK0]),skolemized(Bride),bind(Bride,sK0(Marriage))],[marriage]) ).
    
%----Skolemize Groom, new symbol sK1 recorded here.
fof(groom,plain,
    ! [Marriage] :
      in_love(sK1(Marriage),sK0(Marriage)),
    inference(skolemize,[status(esa),new_symbols(skolem,[sK1]),skolemized(Groom),bind(Groom,sK1(Marriage))],[bride]) ).
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

### Example 2
<pre>
fof(a1, axiom, ![X] : (f(f(X)) = f(g(X)) | g(f(X)) = f(f(X)))).

fof(c, conjecture, g(f(a)) = f(g(a))).

fof(s1, plain, f(f(a)) = f(g(a)), inference(deduction, [status(thm)], [a1])).
fof(s2, plain, f(f(a)) = g(f(a)), inference(deduction, [status(thm)], [a1])).
fof(s3, plain, g(f(a)) = f(g(a)), inference(deduction, [status(thm)], [s1, s2])).
</pre>

### Example 3
<pre>
fof(marriage,plain,
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
      in_love(Groom,Bride) ).

fof(bride,plain,
    ! [Marriage] :
    ? [Groom] :
      in_love(Groom,sK0(Marriage)),
    inference(skolemize,[status(esa),skolemized(Bride),bind(Bride,sK0(Marriage))],[marriage]) ).
    
fof(groom,plain,
    ! [Marriage] :
      in_love(sK0(Marriage),sK0(Marriage)),
    inference(skolemize,[status(esa),new_symbols(skolem,[sK0]),skolemized(Groom),bind(Groom,sK0(Marriage))],[bride]) ).
</pre>

## Example 4
<pre>
fof(marriage,plain,
    ! [Marriage] :
    ? [Bride] :
    ? [Groom] :
      in_love(Groom,Bride) ).

fof(bride,plain,
    ! [Marriage] :
    ? [Groom] :
      in_love(Groom,sK0(Marriage)),
    inference(skolemize,[status(esa),skolemized(Bride),bind(Bride,sK0(Marriage))],[marriage]) ).

fof(groom,plain,
    ! [Marriage] :
      in_love(Marriage,sK0(Marriage)),
    inference(skolemize,[status(esa),new_symbols(skolem,[sK1]),skolemized(Groom),bind(Groom,sK1(Marriage))],[bride]) ).
</pre>