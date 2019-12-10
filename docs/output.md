---
permalink: /output/
nav_order: 17
---

# Output

All output goes directly to the terminal screen.  H. Youngren's plot 
package Xplot11 (libPlt.a) used by XFOIL drives monochrome and color 
X-Windows graphics, and generates B&W or color PostScript files for 
hardcopy.  The default setup assumes color X-Windows graphics 
(if available), and B&W PostScript.  These defaults are controlled 
by the `IDEV` and `IDEVRP` flags in `SUBROUTINE INIT` (in xfoil.f).

The Xplot11 library should work on all Unix systems.  The Makefile
in the ./plotlib/ directory requires some modifications for some 
machines.

The default X-graphics window is in Landscape mode, with a black
(reverse-video) background.  Normal-video can be selected by setting 
the Unix shell variable

```
 % setenv XPLOT11_BACKGROUND white
```

before Xfoil is started.  The nicer reverse-video is restored with:

```
 % unsetenv XPLOT11_BACKGROUND
```

See the plotlib/Doc file for more info on the plot library.

Xplot11 provides a built-in Zoom/Unzoom capability which can
be applied to whatever is on the screen.  Zooming/Unzooming
can be perfomed with the "Z" and "U" commands from nearly all
the menus --- these commands are not listed to reduce clutter.

Some of the menus also have their own Blowup/Reset commands.  
The distinction is that XFOIL's plots don't try to adjust 
themselves to Zoom parameters, so a highly-"Zoomed" plot may 
show nothing at all.  In contrast, Blowup/Reset instructs 
XFOIL to change its own plot scales, so a highly-"Blown-up"
plot will at least show the axes.