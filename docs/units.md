---
nav_order: 9
---

# Units

Most XFOIL operations are performed on the airfoil's cartesian 
coordinates x,y , which do not necessarily have a unit chord c.
Since the chord is ambiguous for odd shapes, the XFOIL 
force coefficients CL, CD, CM are obtained by normalizing the 
forces and moment with only the freestream dynamic pressure
(the reference chord is assumed to be unity).  Likewise, the 
XFOIL Reynolds number RE is defined with the freestream velocity 
and viscosity, and an implied unit chord:

 CL = L / q           |     V = freestream speed
 CD = D / q           |     v = freestream kinematic viscosity
 CM = M / q           |     r = freestream density
 RE = V / v           |     q = 0.5 r V^2

The conventional definitions are

 Cl = L / q c   |
 Cd = D / q c   |
 Cm = M / q c^2 |
 Rc = V c / v   |

so that the conventional and XFOIL definitions differ only by 
the chord factor  c  or  c^2.

For example, a NACA 4412 airfoil is operated in the `OPER` menu at
 
```
 RE   = 500000
 ALFA = 3
```

first with chord=1.0, and then with chord=0.5 (changed with `SCAL` 
command in the `GDES` menu, say).  The results produced by XFOIL are:

```
 c = 1.0 :    CL = 0.80    CD = 0.0082    (RE = 500000, Rc = 500000)
 c = 0.5 :    CL = 0.40    CD = 0.0053    (RE = 500000, Rc = 250000)
```

Since CL is not normalized with the chord, it is nearly proportional
to the airfoil size.  It is not exactly proportional, since the true
chord Reynolds number Rc is different, and there is always a weak 
Reynolds number effect on lift.  In contrast,  the CD for the smaller 
airfoil is significantly greater than 1/2 times the larger-airfoil CD, 
since chord Reynolds number has a significant impact on profile drag.  
Repeating the c = 0.5 case at RE = 1000000, produces the expected 
result that CL and CD are exactly 1/2 times their c = 1.0 values.

 c = 0.5 :    CL = 0.40    CD = 0.0041    (RE = 1000000, Rc = 500000)

Although XFOIL performs its operations with no regard to the size 
of the airfoil, some quantities are nevertheless defined in terms 
of the chord length.  Examples are the camber line shape and BL trip 
locations, which are specified in terms of the relative x/c,y/c along 
and normal to the airfoil chord line.  This is done only for the user's 
convenience.  In the input and output labeling, "x,y" always refer 
to the cartesian coordinates, while "x/c,y/c" refer to the chord-
based coordinates which are shifted, rotated, and scaled so that 
the airfoil's leading  edge is at (x/c,y/c) = (0,0), and 
the airfoil's trailing edge is at (x/c,y/c) = (1,0).  The two 
systems cooincide only if the airfoil is normalized.
