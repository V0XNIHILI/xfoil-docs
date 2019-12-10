---
nav_order: 5
---

# Data Structure
{: .no_toc }

XFOIL stores all its data in RAM during execution. Saving of the data
to files is NOT normally performed automatically, so the user must be
careful to save work results before exiting XFOIL. The exception to
this is optional automatic saving to disk of polar data as it's being
computed in OPER (described later).

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Stored airfoils and polars

XFOIL 6.9 stores multiple polars and associated airfoils and parameters
during one interactive session. Each such data set is designated by its
"stored polar" index:

```
polar 1: x,y, CL(a), CD(a)... Re, Ma, Ncrit...
polar 2: x,y, CL(a), CD(a)... Re, Ma, Ncrit...
.
.
```

Not all of the data need to be present for each stored polar.
For example, x,y would be absent if the CL,CD polar was read in
from an external file rather than computed online.

Earlier XFOIL versions in effect only allowed one stored airfoil
and stored polar at a time. The new multiple storage feature makes
iterative redesign considerably more convenient, since the cases
can contain multiple design versions which can be easily overlaid
on plots.

## Current and buffer airfoils

XFOIL 6.9 retains the concept of a "current airfoil"
and "buffer airfoil" used in previous versions.
These are the airfoils on which the various calculations
are performed, and they are distinct from the "polar" x,y coordinates
described above. The polar x,y are simply archived data,
and do not directly participate in computations. The polar
x,y must first be transferred into the current airfoil if
they are to be used for computation.
