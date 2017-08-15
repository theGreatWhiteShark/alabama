The **alabama** package introducing nonlinear constraints into optimizations using *stats::optim* or *stats::nlminb*. 

Since the original version of the package contains no comments at all, I will share my comments in this repository.

The function **auglag** designed for user interactions call three different wrapper functions performing the calculation:
- auglag1 - for equality constraints only
- auglag2 - for inequality constraints only
- auglag3 - for both equality and inequality constraints

Since all three functions share more or less the same code base (a lot of redundancy), I added comments just to the second one.

# Optimization procedure
The optimization is inspired by the *bound-constrained formulation* in Jorge Nocedal's and Stephen J. Wright's book [Numerical optimization](https://link.springer.com/book/10.1007%2F978-0-387-40065-5) in section 17.4 *practical augmented Lagrangian methods*.

The supplied function will be augmented by the constraints. In two additive terms the squared infeasibilities weighted by a penalty parameter and infeasibilities multiplied by the estimated Lagrangian multiplier will be added to reduce/avoid the violations of the constraints and to avoid systematic perturbations.

An inner routine performing a unconstrained optimization with the augmented Lagrangian using the *stats::optim* or *stats::nlminb* and an outer routine determining the new estimates for both the penalty parameter and the Lagrangian multiplier will be used.

The outer routine works as follows: If the maximal constraint violation is less then a quarter of the one the previous update, the Lagrangian multiplier is updated by subtracting the individual constraint violations times the penalty parameter. If, instead, the constraint violation is larger than a quarter, the penalty parameter will be increased by a factor of ten to put more weight on reducing the violations during the next run of the inner routine.
