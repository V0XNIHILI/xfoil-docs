# Program Execution

XFOIL is executed with

```
 % xfoil
```

When the program starts, the following top level menu and prompt appear: 

```
   QUIT   Exit program

  .OPER   Direct operating point(s)
  .MDES   Complex mapping design routine
  .QDES   Surface speed design routine
  .GDES   Geometry design routine

   SAVE f Write airfoil to labeled coordinate file
   PSAV f Write airfoil to plain coordinate file
   ISAV f Write airfoil to ISES coordinate file
   MSAV f Write airfoil to MSES coordinate file
   REVE   Reverse written-airfoil node ordering

   LOAD f Read buffer airfoil from coordinate file
   NACA i Set NACA 4,5-digit airfoil and buffer airfoil
   INTE   Set buffer airfoil by interpolating two airfoils
   NORM   Buffer airfoil normalization toggle

   BEND   Display structural properties of current airfoil

   PCOP   Set current-airfoil panel nodes directly from buffer airfoil points
   PANE   Set current-airfoil panel nodes ( 140 ) based on curvature
  .PPAR   Show/change paneling

  .PLOP   Plotting options

   WDEF f Write  current-settings file
   RDEF f Reread current-settings file
   NAME s Specify new airfoil name
   NINC   Increment name version number

   Z       Zoom    | (available in all menus)
   U       Unzoom  | 

 XFOIL   c>  
```

The commands preceded by a period place the user in another 
lower-level menu.  The other commands are executed immediately 
and the user is prompted for another top level command.  
The lowercase letters i,r,f,s following some commands indicate 
the type of argument(s) expected by the command:

```
  i  integer
  r  real
  f  filename
  s  character string
```

Commands will be shown here in uppercase, although they are not
case sensitive.

Typically, either the `LOAD` or the `NACA` command is issued first
to create an airfoil for analysis or redesign.  The `NACA` command
expects an integer argument designating the airfoil:

```
 XFOIL  c>  NACA 4415
```

As with all commands, omitting the argument will produce a prompt:

```
 XFOIL  c>  NACA

 Enter NACA 4 or 5-digit airfoil designation   i>  4415
```

The `LOAD` command reads and processes a formatted airfoil coordinate 
file defining an arbitrary airfoil.  It expects a filename argument:

```
 XFOIL  c>  LOAD e387.dat
```

The `NACA` or `LOAD` commands can be skipped if XFOIL is executed with 
a filename as an argument, as for example

```
 % xfoil e387.dat
```

which then executes the `LOAD` procedure before the first menu prompt 
is given.