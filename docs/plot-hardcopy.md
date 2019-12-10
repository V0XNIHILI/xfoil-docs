---
permalink: /plot-hardcopy/
nav_order: 18
---

# Plot Hardcopy

For hardcopy, the current screen plot can be echoed to a PostScript file
plot.ps with the `HARD` command. The size of the plot objects on the
screen and on hardcopy can be changed with the `SIZE` command from most
menus. The number requested is the width of the plot in inches.

**NOTE**

If the plot.ps file is to be previewed with some X-Windows PostScript
viewer, or imported into word-processing systems, XFOIL must be exited
with QUIT in order for the plot.ps file to be properly terminated.  
For just printing, this may or may not be necessary.

**NOTE**

For the geometry plot in `GDES`, and the Qspec(s) plots in `QDES` and `MDES`
(described below), the hardcopy plot size will also be affected if
the graphics window is resized with the cursor at the window manager
level. This is because the plot is always scaled so that it fills
up as much of the window as possible. If the window size is left
at its start-up size, the hardcopy plot widths will come out with the
specified size in inches. If any window dimension is increased from
its default value, then a subsequent hardcopy plot will probably not
fit on a standard 8.5" x 11.0" sheet.
