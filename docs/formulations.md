---
permalink: /formulations/
nav_order: 4
---

# Formulations
{: .no_toc }

Please note that this 'page' originally did not exist in the table of contents, but I combined the
various formulations as presented in there on one page here.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Inviscid

The inviscid formulation of XFOIL is a simple linear-vorticity stream
function panel method.  A finite trailing edge base thickness is modeled
with a source panel.  The equations are closed with an explicit Kutta
condition.  A high-resolution inviscid calculation with the default 
160 panels requires seconds to execute on a RISC workstation.  Subsequent 
operating points for the same airfoil but different angles of attack 
are obtained nearly instantly.

A Karman-Tsien compressibility correction is incorporated, allowing
good compressible predictions all the way to sonic conditions.  The
theoretical foundation of the Karman-Tsien correction breaks down
in supersonic flow, and as a result accuracy rapidly degrades as the
transonic regime is entered.  Of course, shocked flows cannot be
predicted with any certainty.

## Inverse

There are two types of inverse methods incorporated in XFOIL:
Full-Inverse and Mixed-Inverse. The Full-Inverse formulation
is essentially Lighthill's and van Ingen's complex mapping method,  
which is also used in the Eppler code and Selig's PROFOIL code.  
It calculates the entire airfoil geometry from the entire surface
speed distribution. The Mixed-Inverse formulation is simply
the inviscid panel formulation (the discrete governing equations
are identical) except that instead of the panel vortex strengths
being the unknowns, the panel node coordinates are treated as
unknowns wherever the surface speed is prescribed. Only a part
of the airfoil is altered at any one time, as will be described later.  
Allowing the panel geometry to be a variable results in a non-linear
problem, but this is solved in a straightforward manner with
a full-Newton method.


## Viscous

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
