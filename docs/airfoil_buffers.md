---
nav_order: 8
---

# Airfoil buffers
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Buffer airfoil normalization

XFOIL will normally perform all operations on an airfoil with the
same shape and location in cartesian space as the input airfoil.
However, if the normalization flag is set (toggled with the `NORM` 
command), the airfoil coordinates will be immediately normalized 
to unit chord and the leading edge will be placed at the origin.  
A message is printed to remind the user.

## Buffer airfoil generation via interpolation

The `INTE` command is new in XFOIL 6.9, and allows interpolating
or "blending" of airfoils in various proportions.  The interpolation 
is performed as follows:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Airfoils 0 and 1 are defined by their cubic splines,

  x0(s0), y0(s0)       x1(s1), y1(s1)

with the discrete secant arc length parameters s computed from 
the coordinates x(i),y(i):

  s(i) = s(i-1) + sqrt[ (x(i)-x(i-1))^2 + (y(i)-y(i-1))^2 ] 


To perform the interpolation, the discrete s0(i) points are first 
used to define discrete fractional parameter values  s'(i) = 0...1,  
from the Leading Edge to the Trailing Edge:

 s'(i)  =   [s0(i) - s0_LE]/[s0_TE - s0_LE]

The s' values are computed separately on the top and bottom airfoil sides.

These fractional parameter values s' are then used to compute new
spline-parameter values s0,s1 for each airfoil, separately on the
top and bottom sides:

 s0(i)  =  s0_LE  +  s'(i) * [s0_TE - s0_LE]    ;  same as original s0(i)
 s1(i)  =  s1_LE  +  s'(i) * [s1_TE - s1_LE]    ;  same as original s0(i)

The interpolated-airfoil points are then computed by computing x,y
from the splines and interpolating them:

 x_new(i)  =  (1-f) x0(s0(i))  +  f x1(s1(i))
 y_new(i)  =  (1-f) y0(s0(i))  +  f y1(s1(i))


Treating the top and bottom surface separately ensures that the 
leading edge point of the new airfoil is the interpolated result 
of the exact 0 and 1 leading edges.  
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

The polar shape of an interpolated airfoil will often be quite close 
to the interpolated polars of its two parent airfoils.  Extrapolation 
can also be done by specifying a blending fraction outside
the 0..1 range, although the resulting airfoil may be quite
weird if the extrapolation is excessive.

A good way to use `INTE` is to "augment" or "tone down" the
modifications to an airfoil performed in `MDES` or `GDES`.
For example, say airfoil B is obtained by modifying airfoil A:

```
 A -> MDES -> B
```

Suppose the modification changed A's polar in the right direction,
but not quite far enough.  The additional needed change can be
done by extrapolating past airfoil B in `INTE`:

Airfoil "0": A
Airfoil "1": B
Interpolating fraction 0..1 :  1.4
Output airfoil: C

Plotted along the "modification axis", the airfoils are:

A         B     C
0.0       1.0   1.4  ...

So airfoil C has 40% more of the change received by B in the redesign.  
Aifoil C's polar will also be changed about 40% more as intended.

## Airfoil node distribution matching

The `INTE` command can be used to impose one airfoil's node distribution
onto another airfoil shape.  This is done as follows:

Airfoil 0:  airfoil providing the node distribution ( s'(i) values )
Airfoil 1:  airfoil providing the shape ( x, y  values )
Interpolating fraction:  1.0

The resulting airfoil will have the shape of airfoil 1, but the
node distribution of airfoil 0.

## Further buffer airfoil manipulation

The `GDES` facility allows very extensive manipulation of the buffer airfoil.
This will be described in much more detail in a later section.  If only
analysis is performed, the `GDES` facility would not normally be used.

## Generation of current airfoil

When the buffer airfoil coordinates are read from a file during startup,
or read in via the `LOAD` command, they are by default 
also copied directly into the ``current'', or working airfoil.  
Hence, no special action is needed to start analysis operations.  However, 
if the input airfoil has a poor point distribution (too many, too few, 
poorly spaced, etc), one can use `PANE` to create a better panel node 
distribution for the current airfoil on the splined buffer airfoil shape.  
The paneling routine increases the point density in areas of 
high curvature (i.e. the leading edge) and at the trailing edge 
to a degree specified by the user.  The user can also increase 
panel density over one additional interval on each airfoil side, 
perhaps near transition.  The current-airfoil paneling can be 
displayed and/or modified with `PPAR`. 

In some cases it is desirable to explicitly re-copy the buffer 
airfoil into the current airfoil via `PCOP`.  In previous XFOIL
versions this had to be done with the equivalent command sequence

```
 LOAD
 GDES
 EXEC
```

With XFOIL 6.9, the `GDES`,`EXEC` commands after `LOAD` are now superfluous.

The `NACA` command automatically invokes the paneling routine to create 
a current airfoil with a suitable paneling.

## Saving current airfoil coordinates

A coordinate file in any one of these four formats can be written 
with the `PSAV`, `SAVE`, `ISAV`, or `MSAV` command, respectively.
When issuing the MSAV command, the user is also asked which element
in the file is to be overwritten.  XFOIL can thus be used to easily
"edit" individual elements in MSES multielement configurations.
Of course, normalization should not be performed on an element if 
it is to be written back to the same multielement file.
Only the current-airfoil coordinates can be saved to a file.
If the buffer or polar x,y coordinates need to be saved, they
must first be copied into the current airfoil.