# Airfoil file formats

LOAD recognizes four airfoil file formats:  Plain,  Labeled,  ISES,  MSES.
All data lines are significant with the exception of lines beginning
with "#", which are ignored.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Plain coordinate file

This contains only the X,Y coordinates, which run from the 
trailing edge, round the leading edge, back to the trailing edge 
in either direction:

```
X(1)  Y(1)
X(2)  Y(2)
 .     .
 .     .
X(N)  Y(N)
```

## Labeled coordinate file

This is the same as the plain file, except that it also has an 
airfoil name string on the first line:

```
NACA 0012
X(1)  Y(1)
X(2)  Y(2)
 .     .
```

This is deemed the most convenient format to use.
The presence of the name string is automatically recognized if
it does not begin with a Fortran-readable pair of numbers.  Hence, 
"00 12 NACA Airfoil" cannot be used as a name, since the "00 12" 
will be interpreted as the first pair of coordinates.  "0012 NACA"
is OK, however.

Some Fortran implementations will also choke on airfoil names 
that begin with T or F.  These will be interpreted as logical
variables, defeating the name-detection logic.  Beginning the
name with _T or _F is a workable solution to this "feature".

## ISES coordinate file

This has four or five ISES grid domain parameters in addition to the name:

```
NACA 0012
-2.0 3.0 -2.5 3.0
X(1)  Y(1)
X(2)  Y(2)
 .     .
```

If the second line has four or more numbers, then these are interpreted
as the grid domain parameters.

## MSES coordinate file

This is the same as the ISES coordinate file, except that it can
contain multiple elements, each one separated by the line

```
999.0 999.0
```

The user is asked which of these elements is to be read in.  