---
permalink: /plotting/
nav_order: 13
---

# Plotting
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Polar calculations and plotting

The polar calculation facility driven from the OPER menu deserves
a detailed description. It has been considerably upgraded from
previous XFOIL versions.

The simplest way to create a polar is to issue the PACC command
which sets the auto polar accumulation toggle and asks for the
optional save and dump filenames. If either filename is
given, each computed operating point will be stored internally
and also written to the specified file. If no filename is given,
the automatic writing is not performed.

The polar's operating points can be computed individually with ALFA,
or more conveniently en masse with ASEQ. One can also use CL or CSEQ,
although these will not work close to CLmax.

The polar can be plotted anytime with PPLO. If previous polars have been
computed or read in with PGET, they can be plotted as well. If a polar
is deemed incomplete, additional points can be computed as needed.

If automatic writing of a polar was not chosen (no filename was given
for PACC), the polar can be written later all at once with the PWRT
command. The only drawback to this approach is that if the program
crashes during a polar calculation sweep for whatever reason, the
computed polar and all other stored information will be lost.

If existing filenames are given to PACC, the subsequent computed
points will be appended to these files, but only if the airfoil name
and flow parameters in the file match the current parameters.
This is to prevent clobbering of the polar file with "wrong"
additional points. Messages are always produced informing
the user of what's going on.

## Off-line polar plotting

Polar save file(s) can also be plotted off-line with the separate
program PPLOT. This is entirely menu driven, and is simply executed:

```
 % pplot
```

The file pplot.def contains plotting parameters, and is read
automatically if available. If it's not available, then internal
defaults are used.

Like the RGET,FREF commands in OPER, PPLOT permits reference data
to be overlaid. A reference polar data file has the following form:

```
CD(1)  CL(1)
CD(2)  CL(2)
 .      .
 .      .
999.0  999.0
alpha(1)  CL(1)
alpha(2)  CL(2)
  .       .
  .       .
999.0   999.0
alpha(1)  Cm(1)
alpha(2)  Cm(2)
  .       .
  .       .
999.0   999.0
Xtr/c(1)  CL(1)
Xtr/c(2)  CL(2)
  .        .
  .        .
999.0   999.0
```

The number of points in each set (CD-CL, alpha-CL, etc.) is arbitrary,
and can be zero.

The contents of a polar dump file can be selectively plotted with
the separate menu-driven program PXPLOT. It is executed with:

```
 % pxplot <dump filename>
```

This allows surface plots of Cp vs x, H vs x, etc. for any or
all of the saved operating points. Of course, these plots can
be generated in XFOIL for any individual operating point, so PXPLOT
and the dump file itself are somewhat redundant in this respect.
