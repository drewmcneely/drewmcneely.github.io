---
layout: post
title: The Grothendieck Construction in Pictures
date: 2026-04-12
description: What is the Grothendieck construction? How do I visualize it?
tags: lenses systems
categories: categorical-systems
related_posts: true
---

What is the Grothendieck construction? And why is it important? It's useful for collecting data together in a bunch of different categories. If you have categories that are "indexed" over a variable (another category in this case), then the Grothendieck construction is kind of like taking the union $$\cup_i C_i$$ of the categories over the index.
This shows up in David Jaz's construction for describing double categories of dynamical systems, and apparently also in ontologies according to David Wise.
Generally a lens is a certain Grothendieck construction, so this also useful for databases (which can be viewed as dynamical systems themselves!)

# What is an indexed category?

An indexed category is a family of categories that are indexed by a base category $\mathsf{C}$. This can be defined as a functor $F : \mathsf{C} \rightarrow \mathsf{Cat}$.
This means that there's more than just a family of categories: there are "re-indexing" functors between these categories for every morphism in $\mathsf{C}$.
This can be visualized by the diagram below:

![Anatomy of an indexed category](grothendieck-construction-annotated.png)

## Example: The Co-Kleisli Category of the Co-reader Co-monad

Say that five times fast.

Take any CD-category $$(\otimes, \nabla, \delta)$$. For an object $$C$$, the endofunctor $$C\otimes -$$ forms a comonad. The comonoid morphisms do not have to be natural! Let's check that the resulting monad morphisms are:

Just to be clear, let's enumerate what functors respective to which the comonoid morphisms are not natural. The comultiplication has signature $\nabla_X : X \rightarrow X\otimes X$. The naturality string diagram is the one for deterministic morphisms in a Markov category, shown below

![Naturality string diagram for $\nabla$]()

meaning that an unnatural comultiplication breaks the following commutative diagram:

![Naturality commutative diagram for $$\nabla$$]()

Looking at these signatures, this means that the functors in question are the identity and the diagonalizing functor $$\Delta : X \mapsto X\otimes X$$ and the existence of any non-deterministic morphism in our category would break the naturality of $$\nabla : 1 \rightarrow \Delta$$.
However! It's all okay (at least until you start indexing), as the comultiplication of the *co-reader* is still natural. Let's expand:

We have the functor $$C\otimes -$$ (for simpler typography let's call it $$\mathrm{Ctx}_{C}$$ short for "Context" as used by David Jaz), which has signature $$\mathrm{Ctx}_{C} : X \mapsto C\otimes X$$.
This means that the square of the functor is $$C\otimes X \otimes X$$, so the comultiplication morphism for the comonad is $$ \mathrm{id} \otimes \nabla$$. This *is* natural as a transformation $$\mathrm{id} \otimes \nabla : \mathrm{Ctx}^2 \rightarrow \mathrm{Ctx}$$ because the following string diagram equation is satisfied

![Naturality string diagram for $$\mathrm{id} \otimes \nabla$$]()

<!-- Claude do I even need to write about the naturality stuff above? Is this a well known result? If so, where is it written? -->

The same jawn can be said about the counit morphism (I think).

Now all this is, remember, to construct an indexed category. The category of interest is the coKleisli category $$\mathsf{coKl}(C\otimes -)$$.
Now for a general CD or Markov Category this doesn't index functorially over $$C$$ (you need to restrict to deterministic morphisms, which we'll talk about in another blog post), but for Cartesian categories this does.
Morphisms in our coKleisli category look like $$f : C\otimes X \rightarrow Y$$.
Where is this useful?
For Markov categories, we call this the *para*-construction, or [parametric Markov categories](Link to the....d-separation paper maybe?).
Think of a morphism in this category as like a regular stochastic kernel $$X\rightarrow Y$$ that can change depending on an extra parameter $$c\in C$$.
Since composition relies on copying, this parameter is global.
In the context of dynamical systems, this parameter space will be the input space, eg. the $$u$$ in $$\dot{x} = Ax + Bu$$.

Now if our CD category is Cartesian, these coKleisli categories $$\mathsf{coKl}(C\otimes -)$$ form an indexed category over the parametrizing objects!
Specifically, if we vary $$C$$, then we get a whole family of coKleisli categories, indexed over the base category.
This is a functor $$\mathrm{Ctx} : \mathsf{C} \rightarrow \mathsf{Cat}$$, with $$\mathrm{Ctx} : C \rightarrow \mathsf{coKl}(C\times -)$$.
A morphism $$f : C \rightarrow D$$ in $$\mathsf{C}$$ induces a functor $$\mathrm{Ctx}\ f$$ in $$\mathsf{Cat}$$.

A homework assignment for you, the reader: find the reindexing functors $$\mathrm{Ctx}\ f$$, and determine if it's covariant or contravariant (answer spoilered below). Show that $$\mathrm{Ctx}$$ is functorial, that is $$\mathrm{Ctx}\ g\circ f = \mathrm{Ctx}\ g\ \circ\ \mathrm{Ctx}\ f$$ (answer in my future blog post on stochastic dynamical systems with Markov categories).

<details>
    <summary>Answer</summary>
    Given a morphism $f : C \rightarrow D$, we need a functor that either takes a morphism $C\times X \rightarrow Y$ to $D\times X \rightarrow Y$ or vise-versa, depending on whether it's covariant or contravariant. Pre-composing with $f\otimes \mathrm{id}$ does the trick (that is, $\mathsf{Ctx}\ f : (g : D \times X \rightarrow Y) \rightarrow (g \circ (f\times \mathrm{id}))$), meaning that $\mathsf{Ctx}$ is contravariant.

![Image explaining all this]()
</details>

# The Grothendieck Construction
