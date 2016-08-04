# Design

## Layout and Grids

- [http://susy.oddbird.net/](http://susy.oddbird.net/)
- [http://breakpoint-sass.com/](http://breakpoint-sass.com/)

visibility: hidden

:   hides the element, but it still takes up space in the layout.

## Typography

[http://type-scale.com/](http://type-scale.com/)

# Selectors

## Adjacent & General sibling combinator

[http://cdpn.io/zCGnc](http://cdpn.io/zCGnc)

## Not Psudoselector

[http://cdpn.io/Ebdav](http://cdpn.io/Ebdav)

# Dimensions

hardware pixel: smallest unit a screen can support. It cannot be
stretched, skewed, or subdivided reference pixel: optically consistent
unit, w3c considers a pixel a reference pixel [^1] density independent
pixel (dip): basically scales pixels up on higher dpi devices so they
are consistent with devices with lower dpi. [^2]

## Vertical Percentages are Relative to Container Width, Not Height

[http://codepen.io/SitePoint/pen/qLnpm](http://codepen.io/SitePoint/pen/qLnpm)

## Media Queries

device-pixel-ratio: media query identifier to determine if pixels are
being scaled.

device-width: identify the screen width width: width of the viewport

If you use em's to represent dimensions then scaling dimensions based on
a devices specs is as simple as changing the base font-size.

# CSS3

## background

```
    background: [background-color] [background-image] [background-repeat]
                [background-attachment] [background-position] / [ background-size]
                [background-origin] [background-clip];
```

[http://codepen.io/SitePoint/pen/qLnpm](http://codepen.io/SitePoint/pen/qLnpm)

## transform

``` css
transform: skew(20deg) scale(1.5); }
transform: rotateX(50deg) scale(2) rotateZ(20deg);
```

## transition

``` css
element { transition: property, property time delay; }
```

## animation

``` css
@keyframes whoosh {
  0% { transform: scale(0.5) translate(0,0); }
  50% { transform: scale(2)  translate(50%,0);  }
  100% {transform: scale(0.5) translate(100%,0)}
}

#animate {animation: whoosh 3s infinite alternate linear;}
```

## flexbox

[http://www.sketchingwithcss.com/samplechapter/cheatsheet.html](http://www.sketchingwithcss.com/samplechapter/cheatsheet.html)

# canvas

## irregular shape rollover

[https://github.com/codepo8/irregular-shape-rollover](https://github.com/codepo8/irregular-shape-rollover)

# SASS

## mixins

``` scss
@mixin bubbles($name) {
    //Stuff Goes Here
}

block {
    @include bubbles($name);
}
```

``` sass
=bubbles($name)
    //Stuff Goes Here

block
    +bubbles($name)
```

## list things

Dictionary Like Rule Creation

``` scss
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
```

The resulting CSS will look like this

``` css
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
```

# Forms

## Number Input

``` html
<input type="number" pattern="[0-9]*" step="any">
```

iOS requires the pattern attribute in order to look like the android
keyboard [^3].

on Desktop you can't controll the keyboard, so a user may add a dollar
sign (which isn't an alpha numeric). step="any" should fix this issue in
Chrome [^4].

An alternative is to just use tel

``` html
<input type="tel">
```

This will give you a cross-device 10-key keypad

# Bootstrap 3

xs = phone sm = tablets md = desktops lg = large desktops

# D3

IE8/IE9 Shim for D3: https://github.com/shawnbot/aight

# SVG

## d - attribute

string which contains a series of path descriptions

path descriptions: - Moveto: "pick up the pen and move it" - Lineto:
moves line from current position to specified location - Curveto: Bezier
curves - Arcto: elliptical curve - ClosePath: draw a straight line from
the current position to the first point in the path

upper-case command: arguments as absolute positions lower-case command:
specify points relative to current position

d works with &lt;path&gt; and &lt;glyph&gt;

# Networking

## Delay packets

add fixed 250ms delay to all outgoing packets on Ethernet interface eth1
[^5]:

``` terminal256
sudo tc qdisc add dev eth1 root netem delay 250ms
```

turn off

``` terminal256
sudo tc qdisc del dev eth1 root netem 
```

## Limit bandwidth

``` terminal256
sudo wondershaper eth1 256 128
```

``` terminal256
sudo wondershaper clear eth1
```

## Inline SVG

An SVG document will look like this:

``` html
<?xml version="1.0" ?>
<svg enable-background="new 0 0 500 500" class="close-icon" version="1.1" viewBox="0 0 500 500" xml:space="preserve" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
```

It's important to note that the `<?xml ?>` line is not required when
embedding the SVG into an HTML document [^6].

    Well-formed XML documents contain only one prolog. So they should be
    removed if you are embedding one SVG inside another.

[^1]: <http://alistapart.com/article/a-pixel-identity-crisis>

[^2]: <http://developer.android.com/guide/practices/screens_support.html#density-independence>

[^3]: <http://www.smashingmagazine.com/2015/05/05/form-inputs-browser-support-issue/>

[^4]: <http://www.smashingmagazine.com/2015/05/05/form-inputs-browser-support-issue/>

[^5]: <http://coreygoldberg.blogspot.com/2015/04/linux-simulating-degraded-network.html>

[^6]: <http://stackoverflow.com/a/27414586/465270>
