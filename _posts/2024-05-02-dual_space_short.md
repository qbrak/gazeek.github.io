---
title: Dual spaces as transposition
date: 2024-05-02 16:40:33 +/- 0001
categories: [Math, Linear Algebra, Intermediate]
tags: [categorization, explainer]     # TAG names should always be lowercase

layout: post

toc: true
mermaid: true
math: true
---

## Introduction

This note tries to explain how to think about elements of dual spaces, in that they work a bit like transposed elements of the original space.

Assuming you know what a [vector space](https://en.wikipedia.org/wiki/Vector_space) and an [inner product](https://en.wikipedia.org/wiki/Inner_product_space) let's define the following:

**Space of linear transformations**<br>
Let $V,W$ be vector spaces over common [field](https://en.wikipedia.org/wiki/Field_(mathematics)) $K$. Then $\text{Hom}(V,W)$ is the set of all linear transformations from $V$ to $W$. 

**Dual space**<br>
For a vector space $V$ over field $K$ we define a dual space $V^{\*}$ as the set of all the linear functionals from $V$ to $K$. Or in symbols:
$$
V^*:=\text{Hom}(V,K)
$$

Why would you want to think about spaces of linear transformations? It seems like an abstract and hard-to-grasp thing...

I will try to show that elements of $ V^{\*}$ are kind of like [transposed](https://en.wikipedia.org/wiki/Transpose) elements of $V$. However, instead of showing how a [functional](https://en.wikipedia.org/wiki/Functional_(mathematics)) (an element of $ V^*:=\text{Hom}(V,K) $) is like a transposed vector, we will show that a vector can be thought of as a linear transformation, or to be more precise, a vector behaves like an element of $\text{Hom}(K,V)$. Intuitively, the idea is that the transposition of a column vector corresponds to the flipping of the roles of the domain and codomain:

$$
\begin{align*}
\left\{
\begin{bmatrix}
\alpha_1 \\
\alpha_2 \\
\vdots \\
\alpha_n
\end{bmatrix} : \alpha_i \in K \right\}&&\rightarrow && \left\{
    [\alpha_1, \alpha_2, ..., \alpha_n]
: \alpha_i \in K \right\} \\
\\
\text{Hom}(K,V)&&\rightarrow && V^*=\text{Hom}(V,K)
\end{align*}
$$

## Vectors as linear transformations

One way to look at a vector is to think that it "holds" information (in $\mathbb{R}^n$ it is a point in space). However, for vector $v$ we can associate a transformation $T_v\in \text{Hom}(K,V)$ in the following way:
$$
T_v(k) :=k \cdot v \text{ for } k \in K
$$

In $\mathbb{R}^n$ you can think of $\text{im}T_v$ (the image of $T_v$) as a line going through the origin and $v$.

<img src="/assets/img/2024-05-02-dual-spaces/dual_graphic.svg" alt="Dual Space" width="400"/>

Let's now investigate a bit more the structure of $V^{\*}$ and $\text{Hom}(K,V)$. If you have trouble here please reference the free book [Linear Algebra Done Right](https://linear.axler.net/) by Sheldon Axler.

<b>Lemma 1.</b> <i>Let $V,W$ be vector spaces over field $K$.Then $\text{Hom}(V,W)$ is a vector space also over field $K$.</i>

*Proof.* Let's prove this by ensuring that [vector space axioms](https://en.wikipedia.org/wiki/Vector_space#Definition_and_basic_properties) are met for $\text{Hom}(V,W)$. We define addition for $f,g\in \text{Hom}(V,W)$ on some element of $v\in V$ as:

$$

(f \underset{\text{Hom}(V,W)}{+} g)(v) = f(v) \underset{W}{+} g(v)

$$

And scalar multiplication as:

$$
(\lambda \underset{\text{Hom}(V,W)}{\cdot} f )(v)=\lambda \underset{W}{\cdot}f(v)
$$

Since both operations are defined using their behavior in $W$ we immediately get associativity, commutativity, and distributivity of scalar multiplication. We also get the additive inverse and $0$ in $\text{Hom}(V,W)$, the latter is a function that is constantly equal to $0_W$. Thus,  $\text{Hom}(V,W)$ is a vector space. $\square$

Since the field $K$ is a vector space itself, we can conclude that both $V^{\*}$ and $\text{Hom}(K,V)$ are vector spaces, with addition and multiplication by scalar defined as in the lemma. 



Now, let's take a $\varphi \in V^*=\text{Hom}(V,K)$ and compose it with $T\in \text{Hom}(K,V)$, so we get:

$$
\Lambda := \varphi \circ T
$$

As a composition of two linear transformations $\Lambda$ is linear and $\Lambda : K \rightarrow K$, thus, $\Lambda \in \text{Hom}(K,K)$. We will show that $\Lambda$ can be associated with the [inner product](https://en.wikipedia.org/wiki/Inner_product_space) $<\cdot , \cdot>$. 

Here is a visual representation $\Lambda$:

<img src="/assets/img/2024-05-02-dual-spaces/dual_graphic_lambda.svg" alt="Lambda visual" width="300"/>

## Numbers as linear transformations

Right now it seems unclear how a linear transformation can be associated with a number. Thus, we need to figure out how to look at numbers differently. One way to think of them is that numbers contain information (e.g. 3 would mean there are 3 dogs in a kennel, or $\sqrt{2}$ kg of wheat). This is the way [natural numbers are defined](https://en.wikipedia.org/wiki/Set-theoretic_definition_of_natural_numbers) when starting from the empty set:

$$
\begin{align*}
   0 & {} := \{\}      && {} = \varnothing,\\
   1 & {} := \{0\}     && {} = \{\varnothing\},\\
   2 & {} := \{0,1\}   && {} = \{\varnothing,\{\varnothing\}\},\\
   3 & {} := \{0,1,2\} && {} = \{\varnothing,\{\varnothing\},\{\varnothing,\{\varnothing\}\}\}.
 \end{align*}
$$

And then, for example, using [equivalence](https://en.wikipedia.org/wiki/Integer#Equivalence_classes_of_ordered_pairs) classes](https://en.wikipedia.org/wiki/Integer#Equivalence_classes_of_ordered_pairs), we can go on to define $\mathbb{Z}$, $\mathbb{Q}$, $\mathbb{R}$, etc....

However, instead of looking at numbers as counting something, we can think of them as linear transformations. For example:
* $0$ is a transformation that transforms everything into itself (like the [Borg](https://en.wikipedia.org/wiki/Borg) XD)
* $1$ is the identity transformation, it keeps everything the same
* $2$ is a transformation that doubles everything (e.g. if you apply $2$ to a cat, you will get double cats \o/)
* etc...

## Showing $\Lambda$ is the inner product

Coming back to the problem of inner products. Knowing this correspondence between numbers and linear transformations we can associate a number $k\in K$ with linear transformation $\Lambda$. The most convenient way to do this is the following:

$$
k = \Lambda(1) = \varphi(T(1))
$$

Great, so it would seem that we can at least associate together $\Lambda$ and a scalar. At least the "types" work for the inner product. Let's go on to show that the value $\Lambda(1)$ equals the inner product.

Now since we have defined $T_w(k)=k\cdot w$ we would **like** to define $v^* \in V^{\*}$, so that the following holds:

$$
(v^*\circ T_w)(1)\stackrel{?}{=}<v,w>=v^Tw
$$

The last equation suggests that $v^* \sim v^T$, however, it only works for finite-dimensional $V$, thus we will limit ourselves to this case from now on. Since $V$ is finite dimensional it has a [basis](https://en.wikipedia.org/wiki/Basis_(linear_algebra)) $B=\\{b_i\\}_{i=1}^n$. 

Using this basis we can define:

$$
\begin{align*}
    \varphi_{b_i}(b_j ) & =
    \begin{cases}
        1 & \text{if } & b_i = b_j \\
        0 & \text{if } & b_i \neq b_j
    \end{cases} \\
\\
    B^* & = \{\varphi_{b_i} : b_i \in B\} \subseteq V^*  \\
\\
    B_T & = \{T_{b_i} : b_i \in B\} \subseteq \text{Hom}(K,V)
\end{align*}
$$

It can be proven that $\varphi_{b_i}$ is well defined and $B^{\*}$ (called the [dual basis](https://en.wikipedia.org/wiki/Dual_basis)), $B_T$ are basis for their respective spaces. I will leave the proof as an exercise for the reader (if you have trouble this is all in [Linear Algebra Done Right](https://linear.axler.net/)).

To define $v^{\*}$ we can start by decomposing $v$ over $B$:

$$
v = \sum_{i=1}^n \alpha_i b_i
$$

Where $b_i$ are elements of base $B$. Then we can define:

$$
v^* := \sum_{i=1}^n \alpha_i \varphi_{b_i}
$$

We can also write:

$$
T_v (k)= kv = \sum_{i=1}^n \alpha_i \cdot k  b_i = \sum_{i=1}^n \alpha_i T_{b_i}(k)
$$

Thus:

$$
T_v = \sum_{i=1}^n \alpha_i T_{b_i}
$$

Putting all this together we get:

$$
[v^*]_{B^*}= [T_v]_{B_T} =[v]_B  = \begin{bmatrix}
\alpha_1 \\
\alpha_2 \\
\vdots \\
\alpha_n
\end{bmatrix}
$$

Since we've defined $v^{\*}$ let's come back to the inner product.

Now, if $v,w\in V$ we can decompose them into:

$$
\begin{align*}
   & v = \sum_{i=1}^n \alpha_i b_i \\
   & w = \sum_{i=1}^n \beta_i b_i \\
\end{align*}
$$

We know that the inner product of $v$ and $w$ should be:

$$
<v,w>= \sum_{i=1}^n \alpha_i \beta_i
$$

Using our functional composition definition we get (the step-by-step equality is discussed below):

$$
\begin{align*}
(v^*\circ T_w)\stackrel{1}{=}\left(v^*\circ\left(\sum_{j=1}^n \beta_j T_{b_j}\right)\right)\stackrel{2}{=}\\
\sum_{j=1}^n\beta_j\left(v^*\circ T_{b_i}\right)\stackrel{3}{=} \\

\sum_{j=1}^n\beta_j\left(\left(\sum_{i=1}^n \alpha_i \varphi_{b_i}\right)\circ T_{b_i}\right)\stackrel{4}{=} \\


\sum_{j=1}^n\sum_{i=1}^n\beta_j \alpha_i (\varphi_{b_i}\circ T_{b_j})
\stackrel{5}{=} \\
\sum_{i=1}^n\beta_i \alpha_i \cdot id_K
\end{align*}
$$

Let's break these equalities one-by-one:

1. Comes from the fact that we can decompose $T_w$ in $B_T$.
2. We use the linearity of $v^{\*}$, i.e. $v^{\*}(\alpha u + \beta w)=\alpha v^{\*}(u) + \beta v^{\*}(w) $.
3. We decompose $v^{\*}$ in $B^{\*}$.
4. We use the definition of addition in $V^{\*}$ from the lemma, i.e. for $v^{\*},w^{\*}\in V^{\*}$ we have $(v^{\*}+w^{\*})(u)=v^{\*}(u)+w^{\*}(u)$.
5. We use the fact that:

   $$
   \varphi_{b_i}\circ T_{b_j} = 
    \begin{cases}
        id_K & \text{if } i = j \\
        0 & \text{if } i \neq j
    \end{cases}
   $$

Great so in fact we got what we wanted:

$$
<v,w>= (v^*\circ T_w)(1) = \left(\sum_{i=1}^n \alpha_i \beta_i \cdot id_K\right)(1) = \sum_{i=1}^n \alpha_i \beta_i 
$$

## Generalization to matrices

Without any justification, I would like to finish by saying that this idea of the dual being the transposition also holds for linear transformations and their matrices.
For more info consult [Linear Algebra Done Right](https://linear.axler.net/).

If we have a linear $F : V \rightarrow W$, then we can define a $F^* : W^* \rightarrow V^{\*}$ for every $w^* \in W^{\*}$:

$$
F^*(w^*) = w^* \circ F
$$

If you take a moment, you will see that the types of these objects match. 

And then, if $B_V$ is a basis of $V$ and $B_W$ is a basis of $W$, their corresponding dual bases are $B_{V^{*}} $ of $V^{*}$ and $B_{W^*}$ of $W^{*}$. Then using the notation $m_{B_W}^{B_V}(F)$, to denote the matrix associated with $F$ with bases $B_V$ and $B_W$ we get:

$$
\left(m_{B_W}^{B_V}(F)\right)^T=m_{B_{V^*}}^{B_{W^*}}(F^*)
$$

Thus at least one reason to think about duality is to express inner product and transposition without talking about matrices. Freeing ourselves from matrices allows this approach to generalize well to [Hilbert spaces](https://en.wikipedia.org/wiki/Hilbert_space), where you can't just write a matrix.