Exercises written in bold are ones I think are more important.

# Monoidal Categories

A lot of this is likely beyond the scope of this work, but I think it's still important.
It's also relevant to probability

1. Explore the different structures of monoidal categories.
   For instance, what does it mean for a monoidal category to be^[Some of these may not even exist]

   a. Strong
   b. Weak
   c. Strict
   d. Lax
   e. Cartesian
   f. Semicartesian
   g. Symmetric

2. Understand the coherence conditions in a monoidal category. There are supposedly an infinite number of them. What happens if they're broken?
3. What are monoidal functors? Maybe some of the above words actually apply to functors, or monoidal functors. Explore.
4. Apparently monoidal functors can transport monoidal structures in categories. Explore how this works.
5. Supposedly Cartesian monoidal categories are required for the $\mathsf{Kl}(C\times -)$ construction.
   This is because it makes every object a comonoid object with a copy and delete morphism.
   (This makes every Cartesian monoidal category a Markov category!)
   I think it also makes every morphism deterministic in the Markov category sense, which is required for associativity of the Grothendieck construction.
   Investigate these claims. How do the comonoid morphisms fall out of the Cartesian nature of the category?
   Does the universality have to do with it?
   Are the comonoid morphisms natural?
6. Are the comonoid morphisms natural in a generic Markov category?

# (Co)Monads and (Co)Kleisli Categories

1. Understand (op)lax functors. Are there such thing as strong and weak functors? Also understand full, faithful, all those kinds of functors.
2. Fully understand monads and comonads. What happens if their natural transformations aren't natural? Or they don't follow associativity and unitality? Maybe nothing happens now, but might have implications later.
3. Define monoidal monads. What does it mean to be
   A. Strong
   B. Weak
   C. Commutative
   D. Symmetric (this is equivalent to above, but is defined differently apparently)
4. Define (co)Kleisli categories. Prove that they're associative and unital.
5. What if the underlying category is monoidal, Cartesian, etc.? What structures can be transported to the (co)Kleisli categories? If the underlying category is Cartesian, is the (co)Kleisli category also Cartesian? (Hint, DJM says no. Find out why.)
6. Define the $C\times -$ comonad and its coKleisli category if you want. Is this monad strong? Is it commutative?
7. **Read Section 2.2 of [this paper](https://arxiv.org/abs/2010.07416). It's very brief. Is $C\otimes -$ a monad in a generic Markov category? Okay let's standardize our terminology: Fritz calls not-actually-a-coKleisli-category $\mathsf{CoKl}(W\otimes -)$ construction a _Parametric Markov Category_, and DJM calls $\mathsf{CoKl}(C\times -)$ a _category of maps with context $C$_ as in Definition 2.6.2.1 of his book. But let's call it a _parametric Cartesian category_ for consistency. We know that a Cartesian category is Markov, and so obviously a parametric Cartesian category is just a parametric Markov category, but probably with some extra structure. A parametric Markov category is Markov itself. Is a parametric Cartesian category Cartesian itself? What structures does a parametric Cartesian category have that are missing from a generic parametric Markov category?**

# Indexed Categories

1. **We talked about _covariant_ indexed categories $F:\mathsf{C}\rightarrow\mathsf{Cat}$. But lenses require the _contravariant_ Grothendieck construction, using a contravariant indexed category $F:\mathsf{C}^{op}\rightarrow\mathsf{Cat}$. Here, the reindexing functors are turned around. Redraw the indexed category picture for the contravariant case to see what happens.**
1. What happens if the reindexing functors are (op)lax? What if the indexed category functor itself is (op)lax? In general, what happens to indexed category if you add or remove structure in the functors or in the base category?
1. Can monoidal structures in the base category be transported to an indexed category? What's required to do so?
1. **A parametric Cartesian category is a contravariant indexed category. Find the reindexing functors**
1. **DJM defines probabilistic and nondeterministic dynamical systems with probability monads, and more generically monadic systems or $M$-systems for a monad $M$. He constructs this through the indexed category $C\mapsto \mathsf{BiKl}(C\times -, M)$. Define a biKleisli category.**
1. **_I think this problem might be novel, but I don't know if it's enough to make a publication. Could we turn it into something?_ The problem with the biKleisli construction is that the update maps in the Grothendieck construction are probabilistic, but the readout maps are still deterministic. This doesn't reflect real life, where sensors are noisy. Further, some probability models do not arise from monads, such as $\mathsf{Gauss}$. Can we generalize this indexed category so that the base category is Markov? Is a parametric Markov category an indexed category? My guess is no. Investigate. What about for deterministic morphisms? Can those generate reindexing functors? If so, we could slightly generalize $M$-lenses to Markov lenses.**

# The Grothendieck Construction

**All of the below exercises are important exercises. But I bet some of them are loooonnnnng**

For reference, given a covariant indexed category $F:\mathsf{C}\rightarrow\mathsf{Cat}$, the covariant Grothendieck construction $\int_{C\in\mathsf{C}} F(C)$ has objects $(A\in\mathsf{C}, X\in F(\mathsf{C}))$, and a morphism $(A,X)\rightarrow (B,Y)$ is made of morphisms $(C\ni f: A\rightarrow B, F(B)\ni g : [Ff](X) \rightarrow Y)$.

1. Show that the Grothendieck construction is a category, ie. that it is unital and associative.
1. The covariant pointwise opposite Grothendieck construction instead has morphisms $(C\ni f: A\rightarrow B, F(B)\ni g : Y \rightarrow [Ff](X))$. Define the composition law. (Hint: It's pretty much the exact same as before, just with arrows turned around)
1. Given a contravariant indexed category $F:\mathsf{C}^{op}\rightarrow\mathsf{Cat}$, the contravariant Grothendieck construction $\int^{C\in\mathsf{C}} F(C)$ (we put the "bounds" on top now just to indicate it's contravariant) now has morphisms $(C\ni f: A\rightarrow B, F(A)\ni g : X \rightarrow [Ff](Y))$. Redraw the diagram and define the compositionlaw.
1. We now finally get to the definition of a lens: A lens is the contravariant Grothendieck construction of the pointwise opposite. That is, given $F:\mathsf{C}^{op}\rightarrow\mathsf{Cat}$, $\mathsf{Lens}_{F} := \int^{C\in\mathsf{C}} F(C)^{op}$. Combine the above two concepts to determine the composition law for $F$-lenses.
1. Walk through the composition computation of parametric Cartesian category lenses. Show that it's identical to Definition 1.3.1.2 on page 20 of DJM.
1. Wiring diagrams are lenses. Their indexed category is the coparametric finite set category^[I made these words up, but I hope they make sense]. That is, $WD := \int^{C\in\mathsf{FinSet}} \mathsf{Kl}(C\sqcup -)$. I know exactly what you're thinking: "Wait, Kleisli categories are covariant!!"^[Jk. I wouldn't expect this thought to ever cross any _normal_ person's mind. I only know about it because DJM mentioned it in a talk.] Indeed, $F:\mathsf{FinSet} \rightarrow \mathsf{Cat}$. How can we make it contravariant?
1. What does a morphism actually look like in $WD$? Walk through the composition computation.
