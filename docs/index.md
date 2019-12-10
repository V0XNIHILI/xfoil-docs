---
layout: default
title: Home
nav_order: 1
permalink: /
---

# XFOIL 6.9  User Primer
**THE last update   30 Nov 2001**

Mark Drela, MIT Aero & Astro | Harold Youngren, Aerocraft, Inc.

## General Description

XFOIL is an interactive program for the design and analysis of subsonic
isolated airfoils. It consists of a collection of menu-driven routines
which perform various useful functions such as:

- Viscous (or inviscid) analysis of an existing airfoil, allowing
  - forced or free transition
    - transitional separation bubble(s)
  - limited trailing edge separation
  - lift and drag predictions just beyond CLmax
  - Karman-Tsien compressibility correction
- Airfoil design and redesign by interactive specification of
  a surface speed distribution via screen cursor or mouse. Two
  such facilities are implemented.
  - Full-Inverse, based on a complex-mapping formulation
  - Mixed-Inverse, an extension of XFOIL's basic panel method
    Full-inverse allows multi-point design, while Mixed-inverse allows
    relatively strict geometry control over parts of the airfoil.
- Airfoil redesign by interactive specification of
  new geometric parameters such as
  - new max thickness and/or camber
  - new LE radius
  - new TE thickness
  - new camber line via geometry specification
  - new camber line via loading change specification
  - flap deflection
  - explicit contour geometry (via screen cursor)
- Blending of airfoils
- Drag polar calculation with fixed or varying Reynolds and/or
  Mach numbers.
- Writing and reading of airfoil geometry and polar save files
- Plotting of geometry, pressure distributions, and polars
  (Versaplot-derivative plot package used)

XFOIL is best suited for use on a good workstation. A high-end PC
is also effective, but must run Unix to support the X-Windows graphics.
The source code of XFOIL is Fortran 77. The plot library also
uses a few C routines for the X-Windows interface.
