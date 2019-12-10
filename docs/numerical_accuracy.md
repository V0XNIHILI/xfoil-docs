---
permalink: /numerical-accuracy/
nav_order: 15
---

# Numerical accuracy

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
   {:toc}

## Panel density requirements

If strong separation bubbles are present in a viscous solution, then
it is very important to have good panel resolution in the region of the
bubble(s). The large gradients at a bubble tend to cause significant
numerical errors even if a large number of panels is used. If a separation
bubble appears to be poorly resolved, it is a good idea to re-panel the
airfoil with more points, and/or with points bunched around the bubble
region. The paneling is controlled from the `PPAR` menu. A good rule of
thumb is that the shape parameter Hk just after transition in the bubble
should not decrease by more than 1.0 per point. Likewise, the surface
velocity Ue/Vinf should not change by more than 0.05 per point past
transition, otherwise there may be significant numerical errors in the
drag. The point values can be observed by issuing `SYMB` from the `VPLO` menu.
Moderate chord Reynolds numbers (1-3 million, say) usually require the
finest paneling, since the bubbles are still important, but very small.
On many airfoils, especially those with small leading edge radii,
the development of the small bubble which forms just behind the
leading edge can have a significant effect on CLmax. For such cases,
the default paneling density at the bubble may not be adequate.
In all cases, inadequate bubble resolution results in a "ragged"
or "scalloped" CL vs CD drag polar curve, so fortunately this is
easy to spot.

## Differencing order of accuracy

The BL equations are normally discretized with two-point central
differencing (i.e. the Trapezoidal Scheme), which is second-order
accurate, but only marginally stable. In particular, it has problems
with the relatively stiff shape parameter and lag equations at
transition, where at high Reynolds number the shape parameter must
change very rapidly. Oscillations and overshoots in the shape parameter
will occur with the Trapezoidal Scheme if the grid cannot resolve this
rapid change. To avoid this nasty behavior, upwinding must be introduced,
resulting in the Backward Euler Scheme, which is very stable, but has
only first-order accuracy. Previous versions of XFOIL allowed a specific
constant amount of upwinding to be user-specified. Currently,
XFOIL automatically introduces upwinding into the equations only
in regions of rapid change (typically transition). This ensures
that the overall scheme is stable and as accurate as possible.

Since only a minimal amount of upwinding is introduced in the
interest of numerical accuracy, small oscillations in the shape
parameter H will sometimes appear near the stagnation point if
relatively coarse paneling is used there. These oscillations
are primarily a cosmetic defect, and do not significantly affect
the downstream development of the boundary layer. Eliminating them
by increasing upwiding would in fact produce much greater errors
in the overall viscous solution.
