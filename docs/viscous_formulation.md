---
nav_order: 6
---

# Viscous Formulation

The boundary layers and wake are described with a two-equation lagged
dissipation integral BL formulation and an envelope e^n transition
criterion, both taken from the transonic analysis/design ISES code.
The entire viscous solution (boundary layers and wake) is strongly
interacted with the incompressible potential flow via the surface
transpiration model (the alternative displacement body model is used
in ISES). This permits proper calculation of limited separation regions.
The drag is determined from the wake momentum thickness far downstream.
A special treatment is used for a blunt trailing edge which fairly
accurately accounts for base drag.

The total velocity at each point on the airfoil surface and wake, with
contributions from the freestream, the airfoil surface vorticity, and
the equivalent viscous source distribution, is obtained from the panel
solution with the Karman-Tsien correction added. This is incorporated
into the viscous equations, yielding a nonlinear elliptic system
which is readily solved by a full-Newton method as in the ISES code.
Execution times are quite rapid, requiring about 10 seconds on a RISC
workstation for a high-resolution calculation with 160 panels. For a
sequence of closely spaced angles of attack (as in a polar), the
calculation time per point can be substantially smaller.

If lift is specified, then the wake trajectory for a viscous calculation
is taken from an inviscid solution at the specified lift. If alpha is
specified, then the wake trajectory is taken from an inviscid solution
at that alpha. This is not strictly correct, since viscous effects will
in general decrease lift and change the trajectory. This secondary
correction is not performed, since a new source influence matrix would
have to be calculated each time the wake trajectory is changed. This
would result in unreasonably long calculation times. The effect of this
approximation on the overall accuracy is small, and will be felt mainly
near or past stall, where accuracy tends to degrade anyway. In attached
cases, the effect of the incorrect wake trajectory is imperceptible.
