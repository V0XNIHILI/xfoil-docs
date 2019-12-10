---
nav_order: 12
---

# Viscous solution acceleration

The execution of a viscous case requires the solution of a large
linear system every Newton iteration. The coefficient matrix of
this system is 1/3 full, although most of its entries are very small.
Substantial savings in CPU time (factor of 4 or more) result when
these small entries are neglected. `SUBROUTINE` `BLSOLV` which solves the
large Newton system ignores any off-diagonal element whose magnitude
is smaller than the variable `VACCEL`, which is initialized in `SUBROUTINE`
`INIT`, and which can be changed at runtime from the `VPAR` menu with the
`VACC` command.

A nonzero `VACCEL` parameter should in principle degrade the convergence rate
of the viscous solution and thus result in more Newton iterations, although
the effect is usually too small to notice. For very low Reynolds number
cases (less than 100000), it MAY adversely affect the convergence rate
or stability, and one should try reducing `VACCEL` or even setting it
to zero if all other efforts at convergence are unsuccessful.

The value of `VACCEL` has absolutely no effect on the final converged
viscous solution (if attained).
