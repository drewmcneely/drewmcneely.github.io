---
layout: post
title: The Grothendieck Construction in Pictures
date: 2026-04-12
description: What is the Grothendieck construction? How do I visualize it?
tags: lenses systems
categories: categorical-systems
related_posts: true
---

What is the Grothendieck construction? 
Well if you read the [n-lab article](https://ncatlab.org/nlab/show/Grothendieck+construction), it's clearly just a reconstruction of a morphism that arises as a pullback along a classifying morphism to some universal object of some universal morphism.[^1]

[^1]: What's the problem?

If you can understand this definition, that's an amazing achievement!
But if you're working with practical applications such as lenses for databases, open dynamical systems, or ontologies, then it might be helpful to see a visual of what's going on.

Before we start, let's get a reminder of one way the Grothendieck construction can be useful: in unifying open dynamical systems theories. Linear continuous-time systems ($\dot{x} = Ax + Bu$), discrete-time systems ($x_{k+1} = f(x_k, u_k)$), and stochastic processes with input ($P(X_{k+1} | X_{k}, U_k)$) all have different update equations, but similar structure. Is there a unifying theory to describe all of these?
With the Grothendieck construction, there is!

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
