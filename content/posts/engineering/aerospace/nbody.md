---
draft: false
title: N-Body Simulation
description: Theory and numerical implementation.

series: [Engineering]
categories: [Aerospace]
tags: [N-Body Simulation]

date: 2022-12-21

ShowToc: true
TocOpen: true

math: true
---

## Theory

### Step 1: Particle Characteristics

Each particle is treated as a point mass, and each point mass has three intrinsic characteristics:

1. a mass \\(m_i\\),
2. a position vector \\(\vec{r}_i=\left[\begin{array}{ccc}x_i & y_i & z_i\end{array}\right]\\), and
3. a velocity vector \\(\vec{v}_i=\left[\begin{array}{ccc}u_i & v_i & w_i\end{array}\right]\\).

### Step 2: Force Calculation

According to Newton's law of universal gravitation, given two distinct point masses in space, the gravitational force acting on the objects takes the form:

$$F=G\frac{m_1m_2}{r^2}$$

where:

* \\(F\\) is the common force between the masses,
* \\(G\\) is the gravitational constant,
* \\(m_1\\) is the first mass,
* \\(m_2\\) is the second mass, and
* \\(r\\) is the distance between the centers of the masses.

However, this formula is only applicable to isolated two-body systems. Therefore, in order to consider \\(N\\) number of point masses, Newton's law of universal gravitation must be updated. This formulation is called the \\(N\\)-body problem.

From Wikipedia: *The \\(N\\)-body problem considers \\(N\\) point masses \\(m_i,i=1,2,...,N\\) in an inertial reference frame in three-dimensional space \\(\mathbb{R}^3\\) moving under the influence of mutual gravitational attraction.* In other words, the \\(N\\) body problem expands Newton's law of universal gravitation to work with any amount of point masses.

The gravitational force felt on mass \\(m_i\\) by a single other mass \\(m_j\\) is:

$$\vec{F}_i=G\frac{m_im_j}{\left\|\vec{r}_j-\vec{r}_i\right\|^2}⋅\frac{\left(\vec{r}_j-\vec{r}_i\right)}{\left\|\vec{r}_j-\vec{r}_i\right\|}=G\frac{m_im_j\left(\vec{r}_j-\vec{r}_i\right)}{\left\|\vec{r}_j-\vec{r}_i\right\|^3}$$

where:

* \\(F_i\\) is the force felt on mass \\(m_i\\) due to mass \\(m_j\\),
* \\(G\\) is the gravitational constant,
* \\(m_i\\) is the first mass,
* \\(m_j\\) is the second mass, and
* \\(\left\|\vec{r}_j-\vec{r}_i\right\|\\) is the magnitude of the distance between \\(\vec{r}_j\\) and \\(\vec{r}_i\\) induced by the \\(l_2\\) norm.

In order to obtain the equations of motion for each particle in the system, the previous equation must be summed for a total of \\(N\\) times per particle:

$$\vec{F}_i=G\sum_{\substack{j=1\\j\not{}i}}^N\frac{m_im_j\left(\vec{r}_j-\vec{r}_i\right)}{\left\|\vec{r}_j-\vec{r}_i\right\|^3}$$

### Step 3: Acceleration Calculation

Given the summation form of Newton's law of gravitation for the \\(N\\)-body problem, solving for the acceleration of a particle is a trivial task. The discretized vector form of Newton's second law for a mass \\(m_i\\) can be written as:

$$\vec{F}_i=m_i\vec{a}_i→\vec{a}_i=\frac{\vec{F}_i}{m_i}$$

Therefore:

$$\vec{a}_i=G\sum_{\substack{j=1\\j\not{}i}}^N\frac{m_j\left(\vec{r}_j-\vec{r}_i\right)}{\left\|\vec{r}_j-\vec{r}_i\right\|^3}$$

## Numerical Implementation

```python
x = 1
```
