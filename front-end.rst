Layout and Grids
----------------

http://susy.oddbird.net/
http://breakpoint-sass.com/

visibility: hidden
    hides the element, but it still takes up space in the layout.

Typography
----------

http://type-scale.com/

Selectors
=========

Adjacent & General sibling combinator
-------------------------------------

http://cdpn.io/zCGnc

Not Psudoselector
-----------------

http://cdpn.io/Ebdav

Dimensions
==========

hardware pixel: smallest unit a screen can support. It cannot be stretched,
skewed, or subdivided
reference pixel: optically consistent unit, w3c considers a pixel a reference pixel [1]_
density independent pixel (dip): basically scales pixels up on higher dpi
devices so they are consistent with devices with lower dpi. [2]_

Media Queries
-------------

device-pixel-ratio: media query identifier to determine if pixels are being
scaled. 

device-width: identify the screen width
width: width of the viewport

If you use em's to represent dimensions then scaling dimensions based on a
devices specs is as simple as changing the base font-size.

CSS3
====

transform
---------
transform: skew(20deg) scale(1.5); }
transform: rotateX(50deg) scale(2) rotateZ(20deg);

transition
----------
element { transition: property, property time delay; }

animation
---------
@keyframes whoosh {
  0% { transform: scale(0.5) translate(0,0); }
  50% { transform: scale(2)  translate(50%,0);  }
  100% {transform: scale(0.5) translate(100%,0)}
}
#animate {animation: whoosh 3s infinite alternate linear;}

flexbox
-------

http://www.sketchingwithcss.com/samplechapter/cheatsheet.html

canvas
======

irregular shape rollover
------------------------

https://github.com/codepo8/irregular-shape-rollover



SASS
====

mixins
------

.. code-block:: sass

    //SCSS
    @mixin bubbles($name) {
        //Stuff Goes Here
    }

    block {
        @include bubbles($name);
    }

    //SASS
    =bubbles($name)
        //Stuff Goes Here

    block
        +bubbles($name)

Bootstrap 3
===========

xs = phone
sm = tablets
md = desktops
lg = large desktops

.. [1] http://alistapart.com/article/a-pixel-identity-crisis
.. [2] http://developer.android.com/guide/practices/screens_support.html#density-independence
