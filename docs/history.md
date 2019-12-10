---
permalink: /history/
nav_order: 2
---

# History

XFOIL 1.0 was written by Mark Drela in 1986. The main goal
was to combine the speed and accuracy of high-order panel methods
with the new fully-coupled viscous/inviscid interaction
method used in the ISES code developed by Drela and Giles.
A fully interactive interface was employed from the beginning
to make it much easier to use than the traditional batch-type
CFD codes. Several inverse modes and a geometry manipulator
were also incorporated early in XFOIL's development, making
it a fairly general airfoil development system.

Since version 1.0, XFOIL has undergone numerous revisions,
upgrades, hacks, and enhancements. These changes mainly originated
from perceived shortcomings during actual design use, so XFOIL
is now strongly geared to practical airfoil development.
Harold Youngren provided the Xplot11 plot package which
is a vast improvement over the grim Versaplot-type package
used initially. Enhancements and suggestions from Youngren
and other people were also incorporated into XFOIL itself
along the way.

Over the past few years, bug reports and enhancement
suggestions have slowed to practically nil, and so
after a final few enhancements from version 6.8, XFOIL 6.9
is officially "frozen" and being made public. Although
any bugs will likely be fixed, no further development
is planned at this point. Method extensions are being
planned, but these will be incorporated in a completely
new next-generation code.

Note to code developers and code enhancers...
XFOIL does not exactly have the cleanest implementation,
but it isn't too bad considering its vast modification
history. Feel free to muck with the code as you like,
provided everything is done under the GPL agreement.
Drela and Youngren will not be inclined to assist with
any code modifications at this point, however, since we
each have a dozen other projects waiting. So proceed
at your own risk.
