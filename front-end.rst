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

Vertical Percentages are Relative to Container Width, Not Height
----------------------------------------------------------------

http://codepen.io/SitePoint/pen/qLnpm

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

background
----------

::

    background: [background-color] [background-image] [background-repeat]
                [background-attachment] [background-position] / [ background-size]
                [background-origin] [background-clip];

.. Note: the element to which you apply clip must be positioned absolutely

http://codepen.io/SitePoint/pen/qLnpm

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


list things
-----------

Dictionary Like Rule Creation

.. code-block:: scss

    $prefix: skioo;
    $color-list: (
      green #7AEA8B,
      yellow #F6FAA2,
      blue #1902E8,
      red #E80202
    );

    @each $value in $color-list {
      .#{$prefix}-#{nth($value, 1)} {
        font-size: 1em;
      }
    }

The resulting CSS will look like this

.. code-block:: css

    .skioo-green {
      font-size: 1em;
    }

    .skioo-yellow {
      font-size: 1em;
    }

    .skioo-blue {
      font-size: 1em;
    }

    .skioo-red {
      font-size: 1em;
    }

Bootstrap 3
===========

xs = phone
sm = tablets
md = desktops
lg = large desktops


Networking
==========

Delay packets
-------------

add fixed 250ms delay to all outgoing packets on Ethernet interface eth1 [3]_:

.. code-block:: terminal256
    
    sudo tc qdisc add dev eth1 root netem delay 250ms

turn off

.. code-block:: terminal256

    sudo tc qdisc del dev eth1 root netem 

Limit bandwidth
---------------

.. code-block:: terminal256

    sudo wondershaper eth1 256 128

.. code-block:: terminal256

    sudo wondershaper clear eth1


.. [1] http://alistapart.com/article/a-pixel-identity-crisis
.. [2] http://developer.android.com/guide/practices/screens_support.html#density-independence
.. [3] http://coreygoldberg.blogspot.com/2015/04/linux-simulating-degraded-network.html

