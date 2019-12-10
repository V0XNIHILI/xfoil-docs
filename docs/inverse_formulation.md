# Inverse Formulation

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
