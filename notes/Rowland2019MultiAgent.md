## Summary

how to deal with uncertainty in ranking for multiagent general-sum games
- sample complexity guarantees to achieve confidence bounds
- adaptive algorithms to select matchups and achieve your confidence bound
- propogating uncertainty to the rankings

this can be useful if our training requires choosing the next best player to look at

## Intro

Elo only deals with 2-player transitive games
Trueskill generalizes to multiplayer
Nash averaging (Balduzzi et al, 2019) allows non-transitivity, but 2-player constant-sum and Nash equilibria are intractable to compute
alpha-rank (Omidshafei et al, 2019) introduced non-transitive general-sum ranking with evolution

If alpha-rank can give us good rankings, how many samples do we need to get those good rankings?

Looking at k-player, general-sum games

## Sample Complexity

Bounds for 1 - delta confidence on rankings
- pretty complex for finite alpha, infinite alpha is simpler
- relies on num strategies, num players, min diff for a single-player deviation

## Adaptive Sampling

How to best sample a new trial to reach confidence faster

**ResponseGraphUCB**
- sampling scheme S
  - Uniform
  - UniformExhaustive: uniform from unresolved (uncertain) pairs
  - Valence-weighted: proportional to how many unresolved pairs this is a part of
  - Count-weighted: inversely proportional to how many times its already been sampled
- stopping condition C(delta)
  - Hoeffding (UCB)
  - Clopper-Pearson (CP-UCB)
  - relaxed UCB
  - relaxed CP-UCB

## Propogating Uncertainty to Rankings

Don't understand why but obtaining bounds on your ranking is equivalent to PageRank in that it can be reduced to a constrained stochastic shortest path (CSSP) optimization problem.

CSSP is able to be unconstrained

Solve CSSP as a standard control problem, get confidence of your rankings

## Experiments

Three games:
- 2-player zero-sum Bernoulli game
- Soccer meta-game (?)
- Kuhn poker meta-game


Confidence increases with number of samples, and the sampling scheme seems to work. ResponseGraphUCB works just as well on Elo.



## Citation

Rowland, Omidshafiei et al. NeurIPS 2019
