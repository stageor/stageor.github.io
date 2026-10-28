---
title: "K-fold cross-validation: getting an honest read on model performance"
tags: [ml, evaluation]
excerpt: "Why a single train/test split isn't enough, and how k-fold cross-validation gives a more reliable estimate of how a model generalises."
---

Splitting your data once into a training set and a test set is the obvious way to check whether a model generalises. It's also fragile: depending on which rows happen to land in the test set, the measured accuracy can swing noticeably, especially with a small or unevenly distributed dataset. K-fold cross-validation fixes this by evaluating the model several times on different splits and averaging the result.

## The procedure

1. Shuffle the dataset and split it into `k` equally sized folds.
2. For each fold `i` from 1 to `k`:
   - Train the model on all the data except fold `i`.
   - Evaluate it on fold `i`, and record the score.
3. Average the `k` scores to get the final performance estimate.

Every data point gets used for both training and evaluation, just never at the same time, and every fold gets a turn as the held-out set. The result is an estimate of model performance, along with a standard deviation across folds, which tells you how stable that estimate actually is.

## Why this is better than one split

A single train/test split gives you one number with no sense of its variance. If you got a lucky or unlucky split, you'd never know. By evaluating across `k` different splits, cross-validation gives both a mean (the expected performance) and a spread (how much that performance depends on which rows happened to be held out). A model whose cross-validation scores vary wildly across folds is telling you something important: either the dataset is small, noisy, or the model is unstable, and a single train/test number would have hidden all of that behind one confident-looking percentage.

## Choosing k

`k = 5` and `k = 10` are the standard choices, and for good reason, they balance two competing costs:

- **Small `k`** (like 2 or 3) means each training fold is a smaller fraction of the data, so the model is trained on less, which can underestimate performance and increase bias in the estimate.
- **Large `k`** (approaching `n`, the dataset size, which is the leave-one-out case) means each training fold is nearly the full dataset, giving low bias, but now you're training the model `n` times, which gets computationally expensive fast, and the resulting fold scores become highly correlated with each other, which increases the variance of the overall estimate in a different way.

`k = 10` tends to be the sweet spot for typical dataset sizes: enough folds to get a stable estimate, not so many that training time becomes impractical.

## Stratified k-fold

For classification problems, especially with imbalanced classes, plain random folds can end up with a wildly different class balance from one fold to the next, which adds noise to the evaluation that has nothing to do with the model itself. Stratified k-fold fixes this by preserving the overall class proportions within every fold, so a dataset that's 90% class A and 10% class B still has roughly that same 90/10 split inside each fold. This is the default choice for classification tasks, while plain k-fold is fine for regression.

## What it doesn't fix

Cross-validation gives a more reliable estimate of generalisation error, but it doesn't substitute for a proper held-out test set that's never touched during model selection or hyperparameter tuning. The usual pattern is to use k-fold cross-validation on the training data to select hyperparameters and compare model architectures, and keep a separate, untouched test set for the final, one-time evaluation of whichever model wins. Using cross-validation to pick the best model and then reporting the cross-validation score itself as the final number tends to be slightly optimistic, since you've implicitly tuned to that data.
