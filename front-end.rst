Layout and Grids
----------------

http://susy.oddbird.net/
http://breakpoint-sass.com/

Selectors
=========

Adjacent & General sibling combinator
-------------------------------------

http://cdpn.io/zCGnc

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


.. [1] http://alistapart.com/article/a-pixel-identity-crisis
.. [2] http://developer.android.com/guide/practices/screens_support.html#density-independence
