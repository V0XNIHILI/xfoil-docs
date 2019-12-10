---
permalink: /caveats/
nav_order: 99
---


# Caveats

The XFOIL code is not foolproof, and requires some level of aerodynamic
expertise and common sense on the part of the user. Although the
inviscid analysis (`OPERi`), geometry design (`GDES`), and Full-Inverse
(MDES) routines are nearly invulnerable to failure, the Mixed-Inverse
(`QDES`) design routines and especially the viscous analysis (`OPERv`)
routines will fail if a "reasonable" problem is not specified.  
Typical failure scenarios are:

- Viscous Analysis (`OPERv`)
  - Massive separation from excessive airfoil thickness,
    flap deflection, or angle of attack
  - Inherently unsteady flow (von Karman vortex street, etc.)
  - Poor resolution of leading edge pressure spike
  - Poor resolution of small viscous features
    (e.g. separation bubbles)
  - Reynolds number too low
- Mixed-Inverse Surface Speed Design (`QDES`)
  - Re-entrant airfoil shape (negative thickness)

A possible consequence of these occurences is an arithmetic fault
causing program failure. This is unlikely, but it does happen
occasionally. It is therefore a good idea to save any previous work
before an ambitious calculation is attempted.

The following situations may give problems strictly due to numerical
roundoff:

- Excessively small panel(s) somewhere on the airfoil
- Airfoil located too far from origin
- Airfoil too thin

These situations will rarely result in an arithmatic failure,
but will typically result in a "ragged" Cp distribution.
Examine the paneling in the `GDES` menu, making the `GSET` command
if neceesary to set the current paneling. Eliminate excessively
small panels my deleting one or more panel nodes with the
DELP command.

When performing viscous analysis calculations, it is always a good idea
to sequence runs so that alpha does not change too drastically from one
case to another. The Newton solution method always uses the last
available solution as a starting guess for a new solution, and works
best if the change from the old to the new solutions is reasonably
small. For this reason, it is best to perform difficult calculations
(such as past CLmax) by gradually increasing alpha. The `ASEQ` command in
`OPER` is convenient for this. If the user insists on a large change from
one point to another, it is best to force a re-initialization of the
boundary layers with the `INIT` command from the `VPAR` menu in `OPER` before
the radical calculation is performed. `INIT` should always be executed
whenever the viscous solution blows up but the program doesn't crash.

The viscous analysis will execute no more Newton iterations than
set by the current iteration limit each time an `ALFA`, `CL`, etc. command
is issued. If convergence is not achieved within this limit, `ALFA` or `CL`
can be issued as often as needed (most easily with "!"), with another
set of Newton iterations being performed each time. This iteration
limit can be changed from its default value of 10 with the `ITER` command
in `OPER`.

One should always be wary of trusting solutions which show regions
of supersonic flow. Such flows can be reliably predicted only with
a truly nonlinear field method (such as the `MSES` code). As a rule
of thumb, if the maximum Mach number doesn't exceed 1.05 anywhere,
shock losses will be very small, the Cp distributions will be
reasonably accurate, and the drag predicted by XFOIL is likely
to be accurate.
