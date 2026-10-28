---
title: "The multivariate Gaussian, and why the covariance matrix is the whole story"
tags: [statistics, ml]
excerpt: "Extending the normal distribution to many dimensions, and what the mean vector and covariance matrix actually encode."
---

The one-dimensional normal distribution is defined by two numbers: a mean and a variance. The multivariate Gaussian is the natural extension of that to many correlated variables at once, and almost everything interesting about it comes from a single object: the covariance matrix.

## The definition

For a random vector `x` in `d` dimensions, the multivariate Gaussian density is

```
p(x) = 1 / ((2π)^(d/2) |Σ|^(1/2)) * exp( -1/2 (x - μ)ᵀ Σ⁻¹ (x - μ) )
```

where `μ` is the mean vector and `Σ` is the `d × d` covariance matrix. It looks more intimidating than it is: the exponent is just a generalisation of `(x - μ)² / σ²` from the scalar case, and the normalising constant out front just makes the whole thing integrate to 1.

## What the mean vector does

`μ` is nothing new, it's the centre of the distribution in each dimension. If you're modelling height and weight jointly, `μ` is simply the average height and average weight.

## What the covariance matrix actually encodes

This is where the interesting structure lives. `Σ` is symmetric and positive semi-definite, and its entries mean:

- **Diagonal entries** `Σᵢᵢ` are the variance of each individual variable on its own, exactly like the scalar Gaussian's `σ²`.
- **Off-diagonal entries** `Σᵢⱼ` are the covariance between variables `i` and `j`. A positive value means the two variables tend to move together, a negative value means they move in opposite directions, and zero means they're uncorrelated (though, importantly, uncorrelated doesn't always mean independent for general distributions, it does for jointly Gaussian variables).

Geometrically, `Σ` determines the shape and orientation of the distribution's probability contours. For two dimensions, the contours of equal density are ellipses: the eigenvectors of `Σ` give the axes of the ellipse, and the eigenvalues give how stretched the ellipse is along each axis. A diagonal `Σ` with equal entries gives circular contours; a diagonal `Σ` with unequal entries gives an ellipse aligned with the coordinate axes; a full `Σ` with nonzero off-diagonal terms gives an ellipse tilted at an angle, reflecting the correlation between variables.

## The Mahalanobis distance

The term `(x - μ)ᵀ Σ⁻¹ (x - μ)` in the exponent is called the Mahalanobis distance, and it's worth sitting with. It's a distance from `x` to the mean that accounts for the scale and correlation of each dimension, so a point that's far from the mean along a direction where the data has low variance counts as "more unusual" than a point the same Euclidean distance away along a direction where the data is naturally spread out. This is exactly why the Mahalanobis distance, rather than plain Euclidean distance, shows up in anomaly detection and in classifiers like Gaussian discriminant analysis.

## Why this shows up everywhere

A few properties make the multivariate Gaussian the default building block for continuous probabilistic models:

- It's fully specified by just `μ` and `Σ`, no higher moments needed.
- Any marginal distribution of a subset of its variables is still Gaussian.
- Any linear transformation of a Gaussian vector is still Gaussian, with an easily computed new mean and covariance.
- Conditioning on some of the variables (for example, given `x₁`, what's the distribution of `x₂`?) also produces a Gaussian, with a closed-form mean and covariance.

That last property, closed-form conditionals, is the reason it underlies Gaussian processes, Kalman filters, and Gaussian mixture models: whenever you need to update a belief about continuous variables given new observations and keep the math tractable, the multivariate Gaussian is usually the first tool reached for.
