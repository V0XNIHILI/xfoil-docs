---
permalink: /analysis/
nav_order: 14
---

# Analysis

Most of the commands in the top level XFOIL menu merely put the user 
into some lower command level with its own menu and prompt.  Issuing
The `OPER` command, for instance, will produce the prompt

```
 .OPERi  c>
```

Typing a " ? " will result in the `OPER` analysis menu being displayed:

```
   <cr>     Return to Top Level
   !        Redo last ALFA,CLI,CL,ASEQ,CSEQ,VELS

   Visc r   Toggle Inviscid/Viscous mode
  .VPAR     Change BL parameter(s)
   Re   r   Change Reynolds number
   Mach r   Change Mach number
   Type i   Change type of Mach,Re variation with CL
   ITER     Change viscous-solution iteration limit
   INIT     Toggle BL initialization flag

   Alfa r   Prescribe alpha
   CLI  r   Prescribe inviscid CL
   Cl   r   Prescribe CL
   ASeq rrr Prescribe a sequence of alphas
   CSeq rrr Prescribe a sequence of CLs

   SEQP     Toggle polar/Cp(x) sequence plot display
   CINC     Toggle  minimum Cp  inclusion in polar
   HINC     Toggle hinge moment inclusion in polar
   Pacc i   Toggle auto point accumulation to active polar
   PGET f   Read new polar from save file
   PWRT i   Write polar to save file
   PSUM     Show summary of stored polars
   PLIS i   List stored polar(s)
   PDEL i   Delete stored polar
   PSOR i   Sort stored polar
   PPlo ii. Plot stored polar(s)
   APlo ii. Plot stored airfoil(s) for each polar
   ASET i   Copy stored airfoil into current airfoil
   PREM ir. Remove point(s) from stored polar
   PNAM i   Change airfoil name of stored polar
   PPAX     Change polar plot axis limits

   RGET f   Read new reference polar from file
   RDEL i   Delete stored reference polar

   GRID     Toggle Cp vs x grid overlay
   CREF     Toggle reference Cp data overlay
   FREF     Toggle reference CL,CD.. data display

   CPx      Plot Cp vs x
   CPV      Plot airfoil with pressure vectors (gee wiz)
  .VPlo     BL variable plots
  .ANNO     Annotate current plot
   HARD     Hardcopy current plot
   SIZE r   Change plot-object size
   CPMI r   Change minimum Cp axis annotation

   BL   i   Plot boundary layer velocity profiles
   BLC      Plot boundary layer velocity profiles at cursor
   BLWT r   Change velocity profile scale weight

   FMOM     Calculate flap hinge moment and forces
   FNEW rr  Set new flap hinge point
   VELS rr  Calculate velocity components at a point
   DUMP f   Output Ue,Dstar,Theta,Cf vs s,x,y to file
   CPWR f   Output x vs Cp to file
   CPMN     Report minimum surface Cp
   NAME s   Specify new airfoil name
   NINC     Increment name version number
```

The commands are not case sensitive.  Some commands expect multiple 
arguments, but if the arguments are not typed, prompts will be issued.

The most commonly-used commands have alternative short forms, 
indicated by the uppercase part of the command in the menu list.
For example, the menu shows...

```
   Alfa r   Prescribe alpha
   CLI  r   Prescribe inviscid CL
   Cl   r   Prescribe CL
   ASeq rrr Prescribe a sequence of alphas
   CSeq rrr Prescribe a sequence of CLs
```

The "A" command is the short alternative form of "ALFA", and "C" 
is the short alternative of "CL".  Likewise, "AS" and "CS"
are the short forms of "ASEQ" and "CSEQ".  The CLI command 
has no short form (as indicated by all capitals in the menu), 
and must be fully typed.

Hopefully, most of the commands are self-explanatory.  For inviscid 
cases, the `CLI` and `CL` commands are identical.  For viscous cases, 
`CLI` is equivalent to specifying alpha, this being determined a priori 
from the specified lift coefficient via an inviscid solution.  `CL` will 
return a viscous solution with the specified true viscous lift 
coefficient at an alpha which is determined as part of the solution 
(prescribing a CL above CLmax will cause serious problems, however!).
The user is always prompted for any required input.  When in doubt, 
typing a " ? " will always produce a menu. 

After an `ALFA`, `CL`, or `CLI` command is executed, the Cp vs x distribution
is displayed, and can be displayed again at any time with `CPX`.
If the viscous mode is active, the true viscous Cp is shown as a solid
line, and the inviscid Cp at that same alpha is shown as a dashed line.
Each dash covers one panel, so the local dashed line density is also 
a useful visual indicator of panel resolution quality.  If the inviscid
mode is active, only the inviscid Cp is shown as a solid line.

The difference between the true viscous Cp distribution (solid line)
and the inviscid Cp distribution (dashed line) is due to the
modification of the effective airfoil shape by the boundary layers.
This effective airfoil shape is shown superimposed on the actual
current airfoil shape under the Cp vs x plot.  The gap between 
these effective and actual shapes is equal to the local displacement 
thickness delta*, which can also be plotted in the VPAR menu.  
This is only about 1/3 to 1/2 as large as the overall boundary 
layer thickness, which can be visualized via the `BL` or `BLC` commands
which diplay velocity profiles through the boundary layer.
`BL` displays a number of profiles equally spaced around the
airfoil's perimeter, while `BLC` displays profiles at cursor-selected 
locations.  The zooming commands Z, U, may be necessary to better 
see these small profiles in most cases.

If the Cp reference data overlay option is enabled with `CREF`, 
initiating a Cp vs x plot will first result in the user being
prompted for a formatted data file with the following format:

```
x(1)  Cp(1)
x(2)  Cp(2)
 .      .
 .      .
```

The Cp vs x plot is then displayed as usual but with the data overlaid.
If FREF has been issued previously, then numerical reference values
for CL, CD, etc. will be requested and added to the plot next to the
computed values.

Boundary-layer quantities are plotted from the `VPLO` menu:

```
   H      Plot kinematic shape parameter
   DT     Plot top    side Dstar and Theta
   DB     Plot bottom side Dstar and Theta
   UE     Plot edge velocity
   CF     Plot skin friction coefficient
   CD     Plot dissipation coefficient
   N      Plot amplification ratio
   CT     Plot max shear coefficient
   RT     Plot Re_theta
   RTL    Plot log(Re_theta)

   X  rrr Change x-axis limits
   Y  rrr Change y-axis limits on current plot

   Blow   Cursor blowup of current plot
   Rese   Reset to default x,y-axis limits
   SIZE r Change absolute plot-object size
  .ANNO   Annotate plot
   HARD   Hardcopy current plot

   GRID   Toggle grid plotting
   SYMB   Toggle node-symbol plotting
   LABE   Toggle label plotting
   CLIP   Toggle line-plot clipping
```

This menu is largely self-explanatory.  The skin friction
coefficient plotted with the CF command is defined as

                             2
      Cf = tau / 0.5 rho Qinf

This differs from the standard boundary layer theory definition 
which uses the local Ue rather than Qinf for the normalization.
Using the constant freestream reference makes Cf(x) have the
same shape as the physical shear stress tau(x).

The dissipation coefficient CD' (this is NOT the drag coefficient!!!)
is plotted with the CD command.  CD'(x) is proportional to the local
energy dissipation rate due to viscous shear and turbulent mixing.
Hence, it indicates where on the airfoil drag is being created.
It is in fact a much better indicator of drag production than Cf(x),
since Cf does not account for pressure drag.  CD', on the other
hand, accounts for everything.  Its relationship to the total 
profile drag coefficient is simply

      /
 CD = | 2 CD' ds
      /

with the integration performed over both boundary layers and also 
the wake.  It will be seen that if the flow is separated at the 
trailing edge, much of the drag contribution (energy dissipation) 
of CD' occurs in the wake.

As mentioned earlier, all forces are normalized with freestream
dynamic pressure only.  CL, CD, CM are the usual chord-based
definitions only if the airfoil has a unit chord -- in general, 
they will scale with the airfoil's chord.  Also, CM is defined 
about the cartesian point (xref,yref) = (0.25,0.0), which is not
necessarily the airfoil's 1/4 chord point.

## Force calculation

The lift and moment coefficients CL, CM, are calculated by direct
surface pressure integration:

            /     _                   /
 CL = L/q = | Cp dx        CM = M/q = | -Cp [(x-xref) dx + (y-yref) dy]
            /                         /

          _
  where   x = x cos(a) + y sin(a)     ; a = angle of attack
          _ 
          y = y cos(a) - x sin(a)

The integrals performed in the counterclockwise direction
around the airfoil contour.  The pressure coefficient Cp is 
calculated using the Karman-Tsien compressibility correction.

The drag coefficient CD is obtained by applying the Squire-Young
formula at the last point in the wake --- NOT at the trailing edge.

                                          (H+5)/2
    CD = D/q = 2 Theta_i  =  2 Theta (u/V)

 where     Theta = momentum thickness |  
               u = edge velocity      |  at end of wake
               H = shape parameter    |

               V = freestream velocity
         Theta_i = momentum thickness at "downstream infinity"


The Squire-Young formula in effect extrapolates the momentum 
thickness to downstream infinity.  It assumes that the wake behaves 
in a asymptotic manner downstream of the point of application.  
This assumption is strongly violated in the near-wake behind an 
airfoil with trailing edge separation,  but is always reasonable 
some distance behind the airfoil.  Hence, the usual application 
of Squire-Young at the trailing edge is questionable with separation 
present, but its application at the last wake point (typically 
1 chord downstream) is always reasonable.  Also, application at 
the last wake point also results in the formula having a smaller 
effect in any case, since there  u ~ V,  and hence  Theta_i ~ Theta.

In most 2-D airfoil experiments, drag is measured indirectly by 
measuring  2 Theta/c  in the wake, often within one chord of the
airfoil's trailing edge.  For consistency, this should be compared 
to the  Theta  value predicted by XFOIL at the same wake location, 
rather than the "true"  Cd = 2 Theta_i/c  value which is effectively 
at downstream infinity.  In general,  Theta_i  will be smaller 
than  Theta.  In most airfoil drag measurement experiments, this 
difference may amount to the drag measurement being several
percent too large, unless some correction is performed.

In addition to calculating the total viscous CD from the wake
momentum thickness, XFOIL also determines the friction and pressure
drag components CDf,CDp of this total CD.  These are calculated by

          /     _
  CDf  =  | Cf dx              CDp  =  CD - CDf
          /

Here, Cf is the skin friction coefficient defined with the 
freestream dynamic pressure, not the BL edge dynamic pressure
commonly used in BL theory.  Note that CDp is deduced from 
CD and CDf instead of being calculated via surface pressure
integration.  This conventional definition

         /     _
 CDp  =  | Cp dy    
         /

is NOT used, since it is typically swamped by numerical noise.

## Transition criterion

Transition in an XFOIL solution is triggered by one of two ways:

  free  transition:  e^n criterion is met
 forced transition:  a trip or the trailing edge is encountered

The e^n method is always active, and free transition can occur
upstream of the trip.  The e^n method has the user-specified 
parameter "Ncrit", which is the log of the amplification factor
of the most-amplified frequency which triggers transition.
A suitable value of this parameter depends on the ambient
disturbance level in which the airfoil operates, and mimics
the effect of such disturbances on transition.  Below are typical 
values of Ncrit for various situations.

     situation             Ncrit
  -----------------        -----
  sailplane                12-14
  motorglider              11-13
  clean wind tunnel        10-12
  average wind tunnel        9     <=  standard "e^9 method"
  dirty wind tunnel         4-8

Note: The e^n method in XFOIL is actually the simplified envelope 
version, which is the same as the full e^n method only for flows 
with constant H(x). If H is not constant, the two methods differ
somewhat, but this difference is typically within the uncertainty
in choosing Ncrit.

The e^n method is only appropriate for predicting transition in 
situations where the growth of 2-D Tollmien-Schlichting waves via
linear instability is the dominant transition-initiating mechanism.  
Fortunately, this happens to be the case in a vast majority of airfoil 
applications.  Other possible mechanisms are:

- Crossflow instabilities. These occur on swept wings with significant  
  favorable chordwise pressure gradients.
- Attachment-line transition.  This requires large sweep, large LE
  radius, and a large Reynolds number.  Occurs primarily on big jets.
- Bypass transition.  This occurs in cases with sufficient wall
  roughness and/or large freestream turbulence or vibration levels.
  The linear-instability phase predicted by the e^n method is
  "bypassed", giving relatively early transition.  Usually occurs
  in favorable pressure gradients, while the linear-instability
  mechanism usually dominates in adverse pressure gradients.

If any of these alternative transition mechanisms are present, the
trips must be set to mimick their effect.  The bypass transition
mechanism can be mimicked to some extent by the e^n method by 
setting Ncrit to a small value --- Ncrit=1 or less.  This will 
cause transition just after linear instability begins.  For
very large freestream turbulence or roughness in favorable
pressure gradients, bypass transition can occur before the 
linear instability threshold, and in this case trips will have 
to be set as well.