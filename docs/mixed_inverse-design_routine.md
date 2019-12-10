---
permalink: /mixed-inverse-design-routine/
nav_order: 20
---

# Mixed-Inverse Design Routine (QDES)
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

XFOIL's Mixed-Inverse facility (`QDES`) is useful in certain redesign 
problems where parts of the airfoil cannot be altered under any 
circumstances.  The Mixed-Inverse menu is shown below.

```
   <cr>   Return to Top Level

   QSET   Reset Qspec <== Q

   Modi   Modify Qspec
   MARK   Mark off target segment
   SMOO   Smooth Qspec inside target segment
   SLOP   Toggle modified-Qspec slope matching flag

   eXec i Execute mixed-inverse calculation
   REST   Restore geometry from buffer airfoil
   CPXX   CPxx endpoint constraint toggle

   Plot   Plot Qspec (line) and Q (symbols)
   VISC   Qvis overlay toggle
   REFL   Reflected Qspec overlay toggle

   Blow   Blowup plot region
   Rese   Reset plot scale and origin
   Wind   Plot window adjust via cursor and keys

   SIZE r Change absolute plot-object size
  .ANNO   Annotate plot
   HARD   Hardcopy current plot
```

The `QDES` menu above is intentionally geared for the redesign of a segment
of an existing airfoil (with its surface speed distribution calculated 
previously in `OPER`) rather than the generation of a totally new airfoil. 

## Creation of seed surface speed distribution

When `QDES` is entered, the specified speed distribution Qspec is 
initialized to the current speed distribution Q last set in `OPER`.
If a direct solution for the current airfoil hasn't been calculated yet, 
`QDES` goes ahead and calculates it, using the last-set angle of attack.
If this isn't the desired angle, it can be set in `OPER` using `ALFA`.
QSET can then be used to set Qspec from the current Q distribution.
The `QSET` command can be used anytime later to "reset" Qspec if the
modification has been botched.

## Modification of surface speed distribution

Qspec can be repeatedly modified with the screen cursor and the `MODI`
command, exactly as in `MDES`.  It is also necessary to mark off the 
target segment where the geometry is to be modified with the 
`MARK` command.  

## Generation of new airfoil geometry

`EXEC` modifies the airfoil over the target segment to match Qspec 
there as closely as possible.  The remainder of the airfoil 
geometry is left unaltered.  `EXEC` requests the number of Newton
iterations to be performed in the inverse calculation.  Although as
many as six iterations may be required for convergence to machine zero,
it is _not_ necessary to fully converge a Mixed-Inverse case.  Two 
iterations are usually sufficient to get very close to the new
geometry.  In any case, the new surface speed distribution Q 
which actually results from the inverse calculation will typically 
differ somewhat from the specified distribution Qspec by function 
modes which are added to Qspec.  At least two modes are added, 
with their magnitudes determined by geometric closure requirements 
at the inverse segment endpoints.  As with the `MDES` complex-mapping
routine, the necessary modifications to Qspec will be smallest if
Qspec is modified so that CL (the area under the Qspec(s) curve) is 
roughly preserved.

Issuing `PLOT` after the `EXEC` command finishes will compare the specified 
(Qspec) and resulting (Q) speed distributions.  If extra smoothness 
in the surface speed is required, the `CPXX` command just before `EXEC` 
will enable the addition of two additional modes which allow the 
second derivative in the pressure at the endpoints to be unchanged 
from the starting airfoil.  The disadvantage of this option is that 
the resulting surface speed Q will now deviate more from the 
specified speed Qspec.  It is allowable to repeatedly modify Qspec, 
set or reset the `CPXX` option, and issue the `EXEC` command in any order.

The Mixed-Inverse modification is performed on the current airfoil 
directly, in contrast to Full-Inverse which generates the buffer 
airfoil as its output.  In fact, it is important _not_ to issue the 
`PANE` or `PCOP` commands at top level after doing work in the `QDES` menu, 
as the new current airfoil will be overwritten with the old buffer 
airfoil.