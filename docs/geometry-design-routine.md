---
permalink: /geometry-design-routine/
nav_order: 21
---

# Geometry Design Routine

Executing the `GDES` command from the top level menu will put the user
into the `GDES` routine.  It has a rather extensive menu:

```
   <cr>     Return to Top Level
   !        Redo previous command

   GSET     Set buffer  airfoil <== current airfoil
   eXec     Set current airfoil <== buffer  airfoil
   SYMM     Toggle y-symmetry flag

   ADEG r   Rotate about origin (degrees)
   ARAD r   Rotate about origin (radians)
   Tran rr  Translate
   Scal r   Scale about origin
   LINS rr. Linearly-varying y scale
   DERO     Derotate (set chord line level)

   TGAP rr  Change trailing edge gap
   LERA rr  Change leading edge radius

   TCPL     Toggle thickness and camber plotting
   TFAC rr  Scale existing thickness and camber
   TSET rr  Set new thickness and camber
   HIGH rr  Move camber and thickness highpoints
  .CAMB     Modify camber shape directly or via loading

   BEND     Display structural properties of buffer airfoil

   Flap rrr Deflect trailing edge flap

   Modi     Modify contour via cursor
   SLOP     Toggle modified-contour slope matching flag

   CORN     Double point with cursor (set sharp corner)
   ADDP     Add    point with cursor
   DELP     Delete point with cursor
   MOVP     Move   point with cursor

   UNIT     Normalize buffer airfoil to unit chord
   Dist     Determine distance between 2 cursor points
   CLIS     List curvatures
   CPLO     Plot curvatures
   CANG     List panel corner angles
   CADD ri. Add points at corners exceeding angle threshold

   Plot     Replot buffer airfoil
   INPL     Replot buffer airfoil without scaling (in inches)
   Blow     Blowup plot region
   Rese     Reset plot scale and origin
   Wind     Plot window adjust via cursor and keys

   TSIZ     Change tick-mark size
   TICK r   Toggle node tick-mark plotting
   GRID     Toggle grid plotting
   GPAR     Toggle geometric parameter plotting
   Over f   Overlay disk file airfoil

   SIZE r   Change absolute plot-object size
  .ANNO     Annotate plot
   HARD     Hardcopy current plot

   NAME s   Specify new airfoil name
   NINC     Increment name version number
```

## Creating seed buffer airfoil

The first command typically executed is GSET, which sets the temporary
buffer airfoil from the current airfoil.  Sometimes it might be desired 
to operate directly on the coordinates of an already existing buffer
airfoil.  It typically contains coordinates read in from a disk file 
by LOAD at Top Level, or coordinates generated by EXEC from the MDES 
menu, depending on what was done last.  In either of these cases, GSET 
is skipped.  

## Point addition (typ. to Eppler and Selig airfoils)

If the buffer airfoil has an excessively coarse point spacing,
additional points can be added with the CADD command.  Using the
PANE command at top level also does this, but CADD allows the point
addition to be restricted to locations with excessive corner angles
(displayed with CANG), and also to locations which fall within a specified
x-range.  Different spline parameters can also be used to determine 
the inserted spline points.  For example, the command

```
 .GDES  c>  CADD 10.0  2  -0.1 0.2
```

will add spline points adjacent to each existing point whose panel
angle exceeds +/-10 degrees, and only if the added point will fall
within the interval -0.1 < x < 0.2.  The "2" indicates that an 
arclength spline parameter is to be used.  The PANE command will
always use the arclength spline.

Some archived airfoils, notably the Eppler airfoils and some of the 
Selig airfoils have an excessively coarse point spacing around 
the leading edge.  The spacing has apparently been tailored for
a uniform-parameter spline, and often produces a badly shaped 
leading edge with the arclength-parameter spline used in Xfoil.
The following command will insert additional points giving 
a much smoother shape for subsequent analysis.

```
 .GDES  c>  CADD 10.0  1  -0.1 1.1
```

The 10.0 degree angle tolerance can be varied as needed (1/2 of the max 
angle is the default).  The "1" argument (also a default) specifies 
a uniform-parameter spline for the interpolation since this works best 
for Eppler airfoils), and the default x range indicates that the 
entire airfoil is to be treated.  The CADD command can be repeated
to keep reducing the max panel angle, but this may or may not 
improve the smoothness of the resulting splined airfoil.


## Modifying buffer airfoil

Once the buffer airfoil is suitably initialized, most of the GDES 
commands can then be used to modify it.  The resulting new shape 
will usually be replotted immediately in a highlighted color.
The plot can be refreshed anytime with the PLOT command.

Sometimes a sequence of commands is necessary to achieve the desired 
effect.  For instance, suppose an airfoil with the current thickness 
envelope is to be given an entirely new camber line. Issuing TSET and 
hitting a `<cr>` (keep same thickness) and a new camber 
of 0 will result in the current thickness envelope unchanged and the 
current camber eliminated, so that a symmetrical airfoil remains. 
The new camber line can then be added in the CAMB sub-menu:

```
   <cr>    Return to GDES
   TFAC rr Scale existing thickness and camber
   TSET rr Set new thickness and camber
   HIGH rr Move camber and thickness highpoints
   WRTC    Write airfoil camber x/c,y/c to file

   RDAC    Read   added camber  x/c,y/c from file
   SETC    Set    added camber  x/c,y/c from camberline
   INPC    Input  added camber  x/c,y/c from keyboard
   MODC    Modify added camber  x/c,y/c with cursor
   INPP    Input  added loading x/c,DCp from keyboard
   MODP    Modify added loading x/c,DCp with cursor
   SLOP    Toggle modified-camber,dCp slope matching flag
   SCAL r  Scale the added camber
   CLR     Clear the added camber
   ADD     Add added camber to the existing camberline

   DCPL    Toggle DCp plot
   CPLI rr Change DCp axis plot limits

   Blow    Blowup plot region
   Rese    Reset plot scale and origin

   SIZE r   Change absolute plot-object size
  .ANNO     Annotate plot
   HARD     Hardcopy current plot

..CAMB   c>
```

`INPC` takes the new camber line as a sequence of x/c,y/c coordinate
pairs which are splined.  INPP takes a sequence of  x/c,delta(Cp)
pairs instead.  This delta(Cp) (i.e. loading) distribution, defined as 

delta(Cp) = (Cp)lower - (Cp)upper

is then used in Glauert's thin-airfoil relations to define the
x/c,y/c camber line.

With INPC and INPP, a slope discontinuity in y(x) or Cp(x) can 
be specified with two identical consecutive x/c values, which 
prevents splining across this point.  INPP can thus easily 
generate a camber line with a piecewise-linear delta(Cp) loading 
distribution, as for example an  a=0.8 NACA 6-digit airfoil:
 x/c delta(Cp)
 0.0   0.5
 0.8   0.5
 0.8   0.5
 1.0   0.0

This results in a constant   delta(Cp) = 0.5  for  0.0 < x/c < 0.8 , 
then decreasing linearly to  delta(Cp) = 0.0  for  0.8 < x/c < 1.0 

Once a suitable added camber is input, it is added to the 
existing buffer airfoil camber via the ADD command.

The various GDES commands for modifying camber-line, thickness, leading
edge radius, etc. should suffice for most geometry modification tasks.  
If truly frustrated, the user can "draw" the new contour with the MODI 
command, which accepts cursor inputs in the same manner as the MDES 
and QDES procedures.  Slope matching at the modified-piece endpoints 
can likewise be enabled/disabled with the SLOP toggle command.  The 
only important difference is that here the points must be entered 
in consecutive order along the new contour, although one can "erase"
a previously-entered point by clicking on "Erase" or type "e" in the
graphics window.

A point can be doubled with the CORN command.  A doubled point is 
useful wherever a sharp corner is required, such as at a flap break.
Normally, the spline routine enforces slope continuity at all points,
effectively preventing sharp corners.  A doubled point, marked by 
a small diamond symbol on the plot, causes separate splines to be 
generated on each side of the corner, thus allowing the slope 
break.  The doubled point is eliminated by clicking on it after 
issuing the DELP command.  Using DELP on a normal single point will 
delete that point entirely.  

The TGAP command sets the thickness (or gap) of the blunt trailing edge.
The gap "ds" is defined as the distance between the upper and lower
coordinate endpoints:  ds^2 = dx^2 + dy^2.  If the gap is already nonzero,
then the new TE base vector (dx,dy) will have the same orientation as 
the old one, i.e.

  dy|       dy|
  --|    =  --|
  dx|       dx|
     new       old

If the gap is zero to begin with, then the new base vector will be
perpendicular to the trailing edge bisector.  If the base orientation
comes out in an unexpected way, it probably means that the gap was
not *exactly* zero.  The fix is to first set the gap to zero, and
then set it again to the desired value.

After a new gap size is input, a "blending distance/c" will also be
requested.  This controls how rapidly the new TE blends into the
original airfoil, and is essentially the length scale for the blending 
function, which is exponential in x/c.  The limiting values are:

distance/c = 0:  Only the upper- and lower-surface TE points are changed
           = 1:  A linear "wedge" is added or subtracted from the airfoil


## Saving buffer airfoil into current airfoil

Once the desired buffer airfoil is created, a new current airfoil is set
directly from the buffer airfoil with the EXEC command (equivalent to
PCOP at top level).  Alternatively, the new current airfoil can be 
re-paneled from the buffer airfoil with the PANE command at top level.  
The new current airfoil can then be analyzed in OPER.  If the buffer 
airfoil has any doubled corner points, the doubled points will be 
eliminated, but a current-airfoil node will fall exactly on each 
buffer-airfoil corner.