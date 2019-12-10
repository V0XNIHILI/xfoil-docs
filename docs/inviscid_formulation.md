---
nav_order: 4
---

# Inviscid Formulation

The inviscid formulation of XFOIL is a simple linear-vorticity stream
function panel method.  A finite trailing edge base thickness is modeled
with a source panel.  The equations are closed with an explicit Kutta
condition.  A high-resolution inviscid calculation with the default 
160 panels requires seconds to execute on a RISC workstation.  Subsequent 
operating points for the same airfoil but different angles of attack 
are obtained nearly instantly.

A Karman-Tsien compressibility correction is incorporated, allowing
good compressible predictions all the way to sonic conditions.  The
theoretical foundation of the Karman-Tsien correction breaks down
in supersonic flow, and as a result accuracy rapidly degrades as the
transonic regime is entered.  Of course, shocked flows cannot be
predicted with any certainty.