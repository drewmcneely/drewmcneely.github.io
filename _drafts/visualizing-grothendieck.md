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

To set the stage, we start with an indexed category $\mathsf{F}: \mathsf{C} \rightarrow \mathsf{Cat}$ which is a collection of categories indexed over a base category $\mathsf{C}$.
The Grothendieck construction $\int_{\mathsf{C}} \mathsf{F}$ then somehow takes the union of these categories over the indexing category.
We'll start by visualizing what an indexed category actually is, along with an example.

# What is an indexed category?

An indexed category is a family of categories that are indexed by a base category $\mathsf{C}$. This can be defined as a functor $\mathsf{F} : \mathsf{C} \rightarrow \mathsf{Cat}$.
(All our examples will actually be contravariant, but it works either way.)
More explicitly, for every object $a\in \mathsf{C}$, there's a category $F(a)$.
Since $\mathsf{F}$ is a functor into $\mathsf{Cat}$, it also maps to functors between the indexed categories. I call these the "re-indexing" functors: for every morphism $f:a\rightarrow b$ in $\mathsf{C}$, there's a functor $F(f): F(a) \Rightarrow F(b)$.
This functor itself maps objects $F(r) \ni p \mapsto F(f)(p) \in F(b)$.
This can be visualized by the diagram below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grothendieck-construction_annotated.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Anatomy of an indexed category. It's functors all the way down.
</div>

The purple morphisms in $\mathsf{F}(a)$ get mapped to $\mathsf{F}(b)$ through $\mathsf{F}(f)$ and to $\mathsf{F}(c)$ through $\mathsf{F}(g\circ f)$ 
The leftover $\mathsf{F}(b)$ morphisms in green get mapped to $\mathsf{F}(c)$ through $\mathsf{F}(g)$.

## Example: The Co-Kleisli Category of the Co-reader Co-monad

Say that five times fast.

Take any CD-category $$(\mathsf{C}, \otimes, \mathrm{copy}, \mathrm{del})$$: that is, a symmetric monoidal category in which every object is a commutative comonoid. For an object $$u$$, the endofunctor $$u\otimes -$$ forms a comonad $(u\otimes -,\ \kappa = \mathrm{copy}\otimes\mathrm{id}, \epsilon = \mathrm{del}\otimes\mathrm{id})$. Note, $\kappa$ and $\epsilon$ are natural even if $\mathrm{copy}$ and $\mathrm{del}$ are not![^2]

[^2]: Quick exercise, prove this.

The coKleisli category of this comonad can be thought of as a parametrizing construction; a morphism $u\otimes x \rightarrow y$ in the coKleisli category is effectively just a morphism $x\rightarrow y$ that is now dependent on an extra external parameter $u$.
For instance in a switching system, the dynamic model $X_{k+1} \sim p(x_{k+1} | x_{k}, u)$ can change based on the parameter $u$.
For open dynamical systems with control input, $u$ will be the input space, giving $[x^T, u^T]^T \mapsto \dot{x}$ with $\dot{x} = Ax + Bu$.

These morphisms compose through copying: say we wnat to compose a dynamics model $(x_{k}, u) \mapsto x_{k+1}$ and measurement model $(x_{k+1}, u) \mapsto y$, that is

$$x_{k+1} = Ax + Bu$$
$$y = Cx + Du$$

Then the map from $x_k$ to $y$ becomes $y = CAx_k + (CB + D)u$ through the following coKleisli composition

![Picture (1)]()

This current construction has every map parametrized by the same object $u$.
But as we know, different systems in the real world could have different input spaces.
Let's parametrize $\mathsf{coKl}(u\otimes -)$ with respect to $u$: we call this map $\mathsf{Ctx}(u) = \mathsf{coKl}(u\otimes -)$ inspired by [David Jaz](Categorical Systems Theory Definition 2.6.2.1).
When we vary $u$, we get systems with different input/contextualizing parameter spaces.
We want to take the union of each of these coKleisli categories so that all systems live in the same category.
The question is, does $\mathsf{Ctx}$ form an indexed category, ie. is it functorial?
If $\mathsf{C}$ is Cartesian, then it is!
It's actually a contravariant indexed category, meaning the re-indexing functors go the other way as in the below picture.

Homework: Find the reindexing functors.

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
    <summary>Click for answer!</summary>
    Given a morphism $f : C \rightarrow D$, we need a functor that either takes a morphism $C\times X \rightarrow Y$ to $D\times X \rightarrow Y$ or vise-versa, depending on whether it's covariant or contravariant. Pre-composing with $f\otimes \mathrm{id}$ does the trick (that is, $\mathsf{Ctx}\ f : (g : D \times X \rightarrow Y) \rightarrow (g \circ (f\times \mathrm{id}))$), meaning that $\mathsf{Ctx}$ is contravariant.

![Image explaining all this]()
</details>


# The Grothendieck Construction
