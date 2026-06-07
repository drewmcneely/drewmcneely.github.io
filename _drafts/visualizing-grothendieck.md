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
But if you're working with applications such as lenses for databases, open dynamical systems, or ontologies, then it might be helpful to see a visual of what's going on.

Before we start, let's get a reminder of one way the Grothendieck construction can be useful: in unifying open dynamical systems theories. Linear continuous-time systems ($\dot{x} = Ax + Bu$), discrete-time systems ($x_{k+1} = f(x_k, u_k)$), and stochastic processes with input ($P(X_{k+1} | X_{k}, U_k)$) all have different update equations, but similar structure. Is there a unifying theory to describe all of these?
With the Grothendieck construction, there is!

To set the stage, we start with an indexed category $F: \mathsf{C} \rightarrow \mathsf{Cat}$ that's indexed over a base category $\mathsf{C}$.
The Grothendieck construction $\int_{\mathsf{C}} F$ then somehow takes the union of these categories over the indexing category.
We'll start by visualizing what an indexed category actually is, along with an example.

# What is an indexed category?

An indexed category is a family of categories that are indexed by a base category $\mathsf{C}$. This can be defined as a functor $F : \mathsf{C} \rightarrow \mathsf{Cat}$.
This means that there's more than just a family of categories: there are "re-indexing" functors between these categories for every morphism in $\mathsf{C}$.
This can be visualized by the diagram below:

![Anatomy of an indexed category](grothendieck-construction-annotated.png)

## Example: Parametric Categories, AKA the Co-Kleisli Category of the Co-reader Co-monad

Say that five times fast.

Take any CD-category $$(\mathsf{C}, \otimes, \mathrm{copy}, \mathrm{del})$$, that is a symmetric monoidal category in which every object is a commutative comonoid. For an object $$C$$, the endofunctor $$C\otimes -$$ forms a comonad $(C\otimes -, \kappa, \epsilon)$ with $\kappa = \mathrm{copy}\otimes\mathrm{id}$ and $\epsilon = \mathrm{del}\otimes\mathrm{id}$. Note, $\kappa$ and $\epsilon$ are natural even if $\mathrm{copy}$ and $\mathrm{del}$ are not![^2]

[^2]: Quick exercise, prove this.

The coKleisli category of this comonad can be thought of as a [parametrizing construction](Link to the d-separation paper); a morphism $C\otimes X \rightarrow Y$ in the coKleisli category is effectively just a morphism $X\rightarrow Y$ that is now dependent on an external parameter $c$.
For instance in a switching system, the dynamic model $X_{k+1} \sim p(x_{k+1} | x_{k}, c)$ can change based on the parameter $c$.
For open dynamical systems with control input, $C$ will be the input space, eg. the $$u$$ in $$\dot{x} = Ax + Bu$$.

If we vary $C$ in the functor $C\otimes -$, does this form an indexed category $\mathrm{Para}: C \mapsto \mathsf{coKl}(C\otimes -)$?
If $\mathsf{C}$ is Cartesian, then it does!
Specifically, if we vary $$C$$, then we get a whole family of coKleisli categories, indexed over the base category.
It doesn't work for non-deterministic morphisms, which is important, and we will show this later.

Homework for you: Find the reindexing functors $\mathrm{Para}(f:C\rightarrow D)$.

<!--
![Naturality string diagram for $\nabla$]()

meaning that an unnatural comultiplication breaks the following commutative diagram:

![Naturality commutative diagram for $$\nabla$$]()

Looking at these signatures, this means that the functors in question are the identity and the diagonalizing functor $$\Delta : X \mapsto X\otimes X$$ and the existence of any non-deterministic morphism in our category would break the naturality of $$\nabla : 1 \rightarrow \Delta$$.
However! It's all okay (at least until you start indexing), as the comultiplication of the *co-reader* is still natural. Let's expand:

We have the functor $$C\otimes -$$ (for simpler typography let's call it $$\mathrm{Ctx}_{C}$$ short for "Context" as used by David Jaz), which has signature $$\mathrm{Ctx}_{C} : X \mapsto C\otimes X$$.
This means that the square of the functor is $$C\otimes X \otimes X$$, so the comultiplication morphism for the comonad is $$ \mathrm{id} \otimes \nabla$$. This *is* natural as a transformation $$\mathrm{id} \otimes \nabla : \mathrm{Ctx}^2 \rightarrow \mathrm{Ctx}$$ because the following string diagram equation is satisfied


![Naturality string diagram for $$\mathrm{id} \otimes \nabla$$]()
-->

<!-- Claude do I even need to write about the naturality stuff above? Is this a well known result? If so, where is it written? -->

<details>
    <summary>Answer</summary>
    Given a morphism $f : C \rightarrow D$, we need a functor that either takes a morphism $C\times X \rightarrow Y$ to $D\times X \rightarrow Y$ or vise-versa, depending on whether it's covariant or contravariant. Pre-composing with $f\otimes \mathrm{id}$ does the trick (that is, $\mathsf{Ctx}\ f : (g : D \times X \rightarrow Y) \rightarrow (g \circ (f\times \mathrm{id}))$), meaning that $\mathsf{Ctx}$ is contravariant.

![Image explaining all this]()
</details>

# The Grothendieck Construction
