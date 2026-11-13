---
layout: post
title: The Grothendieck Construction in Pictures Part 1 -- Indexed Categories
date: 2026-11-13 16:30:00-0500
description: What is the Grothendieck construction? How do I visualize it?
tags: lenses systems categories markov-categories controls
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

To set the stage, the spaces and models in our systems theory are organized in an indexed category $\mathsf{F}$; that is, a collection of categories with functors between them indexed by a base category $\mathsf{C}$.
The Grothendieck construction $\int_{\mathsf{C}} \mathsf{F}$ then in a certain sense takes the union of these categories so that maps in different contexts all live together.
We'll start in this post by visualizing what an indexed category actually is, along with an example.

# What is an indexed category?

An indexed set $S_{i}$ is a family of sets, one for each element $i\in I$.
This can be viewed as a map $f: I \rightarrow \mathsf{Set}$ which takes each index to its set $I\ni i \mapsto S_i \in \mathsf{Set}$.

Similarly, an indexed category $\mathsf{F}$ is a family of categories indexed by objects in a base category $\mathsf{C}$.
This means that for every object $a\in \mathsf{C}$, there's a category $\mathsf{F}a$.
We add extra categorical data though: our map is now a functor $\mathsf{F} : \mathsf{C} \rightarrow \mathsf{Cat}$.
This means that not only do objects in $\mathsf{C}$ map to categories, but also morphisms in $\mathsf{C}$ map to functors between those categories.
I call these the "re-indexing" functors: for every morphism $f:a\rightarrow b$ in $\mathsf{C}$, there's a functor $\mathsf{F}f: \mathsf{F}a \Rightarrow \mathsf{F}b$.
Since there are multiple layers of functors, here's a drawing of what's going on:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/visualizing-grothendieck-construction/generic-indexed-category.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Anatomy of an indexed category. It's functors all the way down.
</div>

As we can see, for an object $a$ in $\mathsf{C}$, we have a category $\mathsf{F}a \in \mathsf{Cat}$.
For a morphism $f:a\rightarrow b \in \mathsf{C}$, we have a re-indexing functor $\mathsf{F}f : \mathsf{F}a \Rightarrow \mathsf{F}b$.

Notice that the objects $p$ $q$ and $r$ in the red category $\mathsf{F}a$ get mapped through the functor $\mathsf{F}f$ into the green category $\mathsf{F}b$, which already has its own objects $u$ $v$ and $w$.
The morphisms between $p$ $q$ and $r$ get mapped too, but I don't label the arrows here for the sake of space.

Further, everything in the green category $Fb$ gets mapped into the blue category $\mathsf{F}c$:
The original red objects and morphisms from $Fa$ were carried all the way through $\mathsf{F}g\circ \mathsf{F}f$, whereas the green objects and morphisms that started in $\mathsf{F}b$ are just mapped into $\mathsf{F}c$ through $\mathsf{F}g$.

Contravariant indexed categories (that is, $\mathsf{F} : \mathsf{C}^{op} \rightarrow \mathsf{Cat}$, where the re-indexing functors go backwards) exist as well, as we'll see in our next example.

## Example: The Co-Kleisli Category of the Co-reader Co-monad

Say that five times fast. Here's what it looks like.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/visualizing-grothendieck-construction/cokl.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Anatomy of an indexed category. It's functors all the way down.
</div>

In a CD-category $\mathsf{C}$ and given an object $u$, the coreader functor $u\otimes -$ forms a co-monad with $\kappa = \mathrm{copy}\otimes\mathrm{id}$ and $\epsilon = \mathrm{del}\otimes\mathrm{id}$.
Note, $\kappa$ and $\epsilon$ are natural even if $\mathrm{copy}$ and $\mathrm{del}$ are not![^3]
Its coKleisli category has the same objects as $\mathsf{C}$ but the morphisms are those in $\mathsf{C}$ that are parameterized by the contextualizing object $u$.

[^2]: We can relax this to any Markov category, in a future blog post!

[^3]: Quick exercise, prove this.

This coKleisli category can be thought of as a parametrizing construction; a morphism $u\otimes x \rightarrow y$ in the coKleisli category is effectively just a morphism $x\rightarrow y$ that is now dependent on an extra external parameter $u$.
For instance in a switching system, the dynamic model $X_{k+1} \sim p(x_{k+1} | x_{k}, u)$ can change based on the parameter $u$.
For open dynamical systems with control input, $u$ will be the input space, giving $[x^T, u^T]^T \mapsto \dot{x}$ with $\dot{x} = Ax + Bu$.

These morphisms compose through copying: say we wnat to compose a dynamics model $(x_{k}, u) \mapsto x_{k+1}$ and measurement model $(x_{k+1}, u) \mapsto y$, that is

$$x_{k+1} = Ax + Bu$$

$$y = Cx + Du$$

Then the map from $x_k$ to $y$ becomes $y = CAx_k + (CB + D)u$ through coKleisli composition.

This current construction has every map parametrized by the same object $u$.
But as we know, different systems in the real world could have different input spaces.
Let's parametrize $\mathsf{coKl}(u\otimes -)$ with respect to $u$: we call this map $\mathsf{Ctx}(u) = \mathsf{coKl}(u\otimes -)$ inspired by [David Jaz](Categorical Systems Theory Definition 2.6.2.1).
When we vary $u$, we get systems with different input/contextualizing parameter spaces.
We want to take the union of each of these coKleisli categories so that all systems live in the same category.
The question is, does $\mathsf{Ctx}$ form an indexed category? Is it functorial?
If $\mathsf{C}$ is Cartesian, then it is!
It's actually a contravariant indexed category, meaning the re-indexing functors go the other way from our above definition.

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

</details>

# Conclusion: Leading to the Grothendieck Construction

We would like a category that has all the morphisms parameterized by different control input spaces or parameter spaces put together in a way that's composable.
The Grothendieck construction allows us to union all the categories together over their indices!
In the next post, we'll be looking at precisely how this is done and contextualize it with our coKleisli example -- which as you might have guessed, is the setting for classical open dynamical systems theory!
