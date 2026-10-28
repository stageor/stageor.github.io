---
title: "Genetic algorithms, explained from first principles"
tags: [genetic-algorithms, ml]
excerpt: "How evolution by natural selection becomes a search algorithm: populations, fitness, crossover, and mutation."
---

A genetic algorithm (GA) is a search technique borrowed from biology: instead of computing an answer directly, you evolve one. You start with a population of rough, often random guesses, and repeatedly keep the better ones, combine them, and mutate them, until the population converges on something good. It's a useful tool whenever the search space is large, the fitness landscape is bumpy, and there's no clean gradient to follow.

## The core loop

Every GA runs the same cycle:

1. **Initialise** a population of candidate solutions, usually at random.
2. **Evaluate** each one with a fitness function.
3. **Select** the fitter individuals to be parents.
4. **Crossover** pairs of parents to produce offspring.
5. **Mutate** the offspring slightly, to keep exploring.
6. Replace the population with the new generation, and repeat until the fitness stops improving or a generation limit is hit.

## Encoding a solution

The first real design decision is how to represent a candidate solution as a chromosome. For a classic example like the knapsack problem, a chromosome is just a bit string, one bit per item, where 1 means "include it". For something like evolving a mathematical expression or a small program, you'd use a tree-structured or grammar-based encoding instead, which is what Grammatical Evolution does. The encoding matters a lot, because crossover and mutation only make sense once you've decided what a chromosome looks like.

## Selection

Selection decides which individuals get to reproduce. A few common strategies:

- **Roulette wheel selection**: each individual's chance of being picked is proportional to its fitness.
- **Tournament selection**: pick a few individuals at random, let the fittest one win. Simple, and easy to tune the selection pressure by changing the tournament size.
- **Rank selection**: sort by fitness and select based on rank rather than raw fitness value, which helps when a few individuals dominate the fitness scale.

Tournament selection is usually the easiest to reason about and the hardest to get badly wrong, which is why it tends to be the default.

## Crossover and mutation

Crossover combines two parent chromosomes into one or two children, the hope being that good traits from each parent end up together in the offspring. A single-point crossover just swaps the tail of two bit strings after a random cut point; there are fancier schemes (two-point, uniform) depending on the encoding.

Mutation flips a small part of a chromosome at random, usually with a low probability per gene. Its job isn't to drive the search, it's to keep diversity in the population so the algorithm doesn't converge too early on a mediocre solution. Too much mutation and the GA turns into random search; too little and it gets stuck.

## Why this works

A GA isn't guaranteed to find the global optimum, and it's not trying to. What it's good at is exploring a large, irregular search space without needing a derivative, which makes it a reasonable choice whenever the fitness function is noisy, discontinuous, or defined over a structure (like a program or a schema) rather than a smooth numeric space. That trade-off, giving up a guarantee of optimality for the ability to search where gradient methods can't, is really the whole pitch for evolutionary computation.
