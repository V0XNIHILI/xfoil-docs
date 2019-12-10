---
permalink: /start-up-defaults/
nav_order: 22
---

# Start-up Defaults

XFOIL has hardwired parameters (in subr. `INIT`) controlling the
paneling, plotting, and viscous execution.  Most of these can 
be changed at runtime in the various menus.  To avoid the need 
to change the parameters everytime XFOIL is executed, they
can be saved to the default file  xfoil.def  with the `WDEF`
command at `TOP LEVEL`.  This file has the format:

```
   140       1.0000   0.1500   0.2000 | Npan    PPanel  TErat  REFrat
    1.0000   1.0000   1.0000   1.0000 | XrefS1  XrefS2  XrefP1 XrefP2
   10.0000   0.5500   0.0150   0.8500 | Size    plotAR  CHsize ScrnFr
   11.0000   8.5000   0.0000   0.0000 | Xpage   Ypage   Xmargn Ymargn
  F        T                          | Lcolor  Lcursor
    1.0000  -2.0000  -0.5000          | CPmax   CPmin   CPdel
    0.0900   0.7000                   | XoffAir ScalAir BLUwt
    0.0000   1.5000   0.5000          | CLmin   CLmax   CLdel
    0.0000   0.0200   0.0100          | CDmin   CDmax   CDdel
   -4.0000  10.0000   2.0000          | ALmin   ALmax   ALdel
    0.0000   0.3000   0.1000          | CMmin   CMmax   CMdel
   1         0.0000   0.0100          | MAtype  Mach    Vaccel
   1         0.0000   9.0000          | REtype  Re/10^6 Ncrit
    1.0000   1.0000                   | XtripT  XtripB
```

```
Line  1: Paneling parameters from the PPAR menu  
Line  2: Paneling refinement locations                 
Line  3: Specifies the absolute plot size, the plot aspect ratio,
          and scales the character, number, and symbol heights   
Line  4: Defines page size and page margins, in inches
Line  5: Flags for color PostScript output, cursor screen input
Line  6: Defines the Cp axis annotations               
Line  7: x-offset and scale for airfoil on Cp vs x plot, BL u profile scale
Line  8: CL axis annotations on polar plot
Line  9: CD axis annotations on polar plot
Line 10: alpha axis annotations on polar plot
Line 11: CM axis annotations on polar plot
Line 12: Mach-CL dependence type, Mach number, solution acceleration parameter
Line 13: Re-CL dependence type, Reynolds number, transition parameter 
Line 14: Forced transition x/c locations on top,bottom sides
```

This file will be read at any time with the `RDEF` command, thus
avoiding the manual entry of all the information.