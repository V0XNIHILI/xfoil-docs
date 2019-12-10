---
permalink: /re-mach-dependence/
nav_order: 16
---

# Re, Mach dependence

A few comments are in order on the TYPE command, which allows the 
user to set the dependence of the Mach and Reynolds numbers on CL.  
Any CL-CD polar can be of the following three types:

 Type   parameters held constant       varying      fixed
 ----   ------------------------       -------   -----------
   1    M          , Re            ..   lift     chord, vel.
   2    M sqrt(CL) , Re sqrt(CL)   ..   vel.     chord, lift
   3    M          , Re CL         ..   chord    lift , vel.

* Type 1 corresponds to a given wing at a fixed velocity going over
an angle of attack range, as in a wind tunnel test alpha sweep or 
a sudden aircraft pullup.  This is also the common form for an 
airfoil polar.

* Type 2 corresponds to an aircraft in level flight at a given altitude 
undergoing trim speed changes.  This is the most useful airfoil polar 
form for determining a drag polar for an aircraft at 1-g.  For this case, 
The "Mach number" input with the MACH command is actually interpreted 
as the product  M sqrt(CL), and the "Reynolds number" input with the 
VISC or RE commands is actually interpreted as  RE sqrt(CL).  For a wing 
in level flight, these products can be computed from the following exact 
relations, with  Re  based on the mean chord:

                         1/2                                1/2
              |  2  W/S |                     1  | 2 rho W |
 M sqrt(CL) = | --- --- |       RE sqrt(CL) = -- | ------- |
              | 1.4  p  |                     mu |    AR   |

 W  = weight         p   = ambient pressure
 S  = wing area      mu  = dynamic viscosity
 AR = aspect ratio   rho = ambient air density

* Type 3 corresponds to a wing of "rubber chord" with a given lift 
at a given speed.  This is best used for selecting an optimum CL 
for an airfoil while taking Reynolds number changes into account.  
The product  RE CL  can be computed from the following:

           2 W
  RE CL = ------       b = span = sqrt(S*AR)
          mu V b

Caution must be used with Types 2 and 3 so as to not allow the CL 
to go negative.  In addition, with non-zero Mach and Type 2, 
the CL must not fall below that value which makes Mach exceed 
unity.  Warning messages are printed when these problems occur.