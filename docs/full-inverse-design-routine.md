---
permalink: /full-inverse-design-routine/
nav_order: 19
---

# Full-Inverse Design Routine (MDES)
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

XFOIL's Full-Inverse complex-mapping facility (`MDES`) takes as input 
a speed distribution "Qspec" specified over the entire airfoil surface, 
modifies it somewhat to satisfy the Lighthill constraints, and generates 
a new overall geometry.  First a bit of the underlying theory...

The geometry and the surface velocities can both be computed from 
a set of complex mapping coefficients "Cn" in the form

```
 x + iy  =  z(w;Cn)
 u - iv  =  f(w;Cn,alpha)
```

where w= 0..2*pi is the independent parameter going around the airfoil.
The z and f functions are rather complicated but this is not important
here.  The key to the full-inverse method is that the mapping coefficients 
Cn can be computed from a known contour angle theta(w) = arctan(dy/dz)
OR from a surface speed q(w) = |u-iv|.  The other quantity then follows.
In summary, the operations and their commands are...

 a) Direct  problem:  theta  ->  Cn  ->  u-iv, q          (QSET)
 b) Inverse problem:  Qspec  ->  Cn  ->  x+iy, theta      (EXEC)


## Creation of seed surface speed distribution

MDES performs QSET and sets Qspec = q automatically upon entry 
if Qspec does not exist.  This Qspec is then the starting point
for subsequent design operations.

This default initialization in effect makes MDES a redesign method 
in which the surface speed distribution of an existing airfoil is 
used as a starting point to generate a new speed distribution.  
A ``pure'' design code which requests the entire surface speed 
distribution every time is often less natural to use, since airfoil 
design is invariably an iterative process involving repeated 
analyze/fix cycles.  The MDES menu is shown below.

   <cr>   Return to Top Level
   !      Redo previous command

   INIT   Re-initialize mapping
   QSET   Reset Qspec <== Q
   AQ r.. Show/select alpha(s) for Qspec
   CQ r.. Show/select  CL(s)   for Qspec

   Symm   Toggle symmetry flag
   TGAP r Set new TE gap
   TANG r Set new TE angle

   Modi   Modify Qspec
   MARK   Mark off target segment for smoothing
   SMOO   Smooth Qspec inside target segment
   FILT   Apply Hanning filter to entire Qspec
   SLOP   Toggle modified-Qspec slope matching flag

   eXec   Execute  full-inverse calculation

   Plot   Replot Qspec (line) and Q (symbols)
   VISC   Qvis overlay toggle
   REFL   Reflected Qspec overlay toggle
   SPEC   Plot mapping coefficient spectrum

   Blow   Blowup plot region
   Rese   Reset plot scale and origin
   Wind   Plot window adjust via cursor and keys

   SIZE r Change absolute plot-object size
  .ANNO   Annotate plot
   HARD   Hardcopy current plot

   PERT   Perturb one Cn and generate geometry


As described above, the initial Qspec distribution is taken from 
"Q", the speed distribution corresponding to the current 
geometry at the last angle of attack employed in OPER.  Qspec 
can be set back to this Q with QSET anytime.


Modification of surface speed distributions
-------------------------------------------

-- Cursor input of modifications --
Qspec can be modified to whatever is desired with the MODI command 
by specifying points with the screen cursor which are then splined.  
The points can be entered in any order.  The last point can be 
erased by clicking on the "Erase" button or simply typing "e"
in the graphics window.  The input sequence is terminated by
clicking on the "Done" button or by typing "d" in the graphics window.
The "Abort" button or typing "a" aborts the MODI command and
returns to the MDES menu.  The BLOW command can be used to enlarge 
regions of interest at any time by specifying opposite corners 
of the blowup region.  

-- Modification endpoint blending --
Normally, the modified piece of Qspec(s) is blended into the current 
Qspec(s) with matching values and slopes at the piece endpoints.  
The slope matching can be turned on/off with the SLOP toggle command.  
If slope matching is turned off, the modified piece will match only 
the existing value, but a slope discontinuity will be allowed.

-- Smoothing --
Qspec can be smoothed with the SMOO command, which normally operates 
on the entire distribution, but can be confined to a target segment 
whose endpoints are selected with the MARK command.  The smoothing 
acts to alleviate second derivatives in Qspec(s), so that with many
consecutive SMOO commands Qspec(s) will approach a straight line
over the target segment.  If the slope-matching flag is set, the 
endpoint slopes are preserved.  

The FILT command is an alternative smoothing procedure which acts on 
the Fourier coefficients of Qspec directly, and is global in its effect.
It is useful for "cleaning up" the entire Qspec(s) distribution if
noise is present from some geometric glitch on the airfoil surface.
Also, unintended noise might be introduced into Qspec from a poor
modification via the cursor.  

FILT acts by multiplying the Fourier coefficients by a Hanning window 
filter function raised to the power of a filter parameter "F".  This 
tapers off the high frequencies of Qspec to varying degrees.  A value 
of F = 0.0 gives no filtering, F = 1.0 gives the standard Hanning filter,
F = 2.0 applies the Hanning filter twice, etc. The standard Hanning 
filter appears to be a bit too drastic, so a filter parameter of 
F = 0.2 is currently used.  Hence, issuing FILT five times corresponds 
to the standard Hanning filter.  The SPEC command displays the mapping 
coefficient spectrum at any time.

-- Symmetry forcing --
The symmetry-forcing option (SYMM toggle) is useful when a symmetric 
airfoil is being designed.  If active, this option zeroes out all
antisymmetric (camber) Qspec changes, and doubles all symmetric 
(thickness) changes.  This unfortunately has the annoying side 
effect of also doubling the numerical roundoff noise in Qspec 
every time a MODI operation is performed.  This noise sooner or later
becomes visible as high-frequency wiggles which double with each 
MODI command.  Issuing FILT occasionally keeps this parasitic 
noise growth under control.

-- Adjustment for Lighthill constraints --
The MODI, BLOW, MARK, SMOO, SLOP, FILT commands can be issued repeatedly 
in any order until Qspec is modified to have the desired distribution.
In general, the surface speed distributions actually plotted will not 
exactly match what was input with the cursor, since corrections are 
automatically added to maintain the specified trailing edge gap and 
to enforce consistency with the freestream speed.  These are known
as the Lighthill constraints on the surface speed.

The trailing edge gap is initialized from the initial airfoil and can 
be changed with TGAP.  To reduce the "corrupting" effect of the 
constraint-driven corrections, a good rule of thumb is that the 
Qspec distribution should be modified so as to preserve the total CL.  
The CL is simply twice the area under the Qspec(s) curve (=  2 x circulation),
so that this area should be preserved.

-- Multipoint surface speed display --
A very useful feature of the MDES facility is the ability to display 
and modify a number of Qspec distributions corresponding to different 
alpha or inviscid CL values.  These values are displayed and/or selected 
via the AQ or CQ commands.  When any one Qspec distribution is modified, 
the result of modification is also displayed on all the other distributions.
This allows rapid design at multiple operating points.  When the Qspec 
curves correspond to specified CL values, the alpha for each curve will be 
adjusted after each Qspec modification so as to preserve that curve's CL.  
The resulting Qspec will therefore not match the input cursor points 
exactly because of this alpha correction.  

Generation of new geometry
--------------------------
The EXEC command generates a new buffer airfoil corresponding 
to the current Qspec distribution.  If subsequent operations on 
this airfoil are to be performed (SAVE, OPER, etc.), it is 
necessary to first generate a current airfoil from this buffer 
airfoil using PANE at the top level menu.  This seemingly 
complicated sequence is necessary because the airfoil points 
generated by EXEC are uniformly spaced in the circle plane, 
which gives a rather poor point (panel node) spacing distribution 
on the physical airfoil.  This sequence also prevents the current
airfoil from being overwritten immediately when EXEC is issued.  
Once the new current airfoil is generated with PANE, it can 
then be analyzed in OPER, modified in GDES, or whatever.

The PERT command allows manual input of the complex mapping
coefficients Cn which determine the geometry.  These coefficients
are normally determined from Qspec(s) (this is the essence of
the inverse method).  The PERT command is provided simply
as a means of allowing generation of geometric perturbation
modes, possibly for external optimization or whatever.

The manually-changed Cn values result in changes in geometry
as well as the current Qspec(s) distributions.  The QSET command
will restore everything to its unperturbed state.

The Full-Inverse facility is very fast, after an initialization 
calculation of several seconds (on a RISC workstation), it requires 
only a fraction of a second to generate the new buffer airfoil.  