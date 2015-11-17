
Search for line in files::
    find ./ -type f -print0 | xargs -0 grep -l "Oh Hai"

xrectsel
--------

xrectsel_ gets x/y offset and size of
rectangular selection::

    > xrectsel
    1196x382+173+180

.. _xrectsel: https://github.com/lolilolicon/xrectsel

GIF screen capture
------------------

::

    byzanz-record --duration=15 --x=200 --y=300 --width=700 --height=400 out.gif


If you want to replace all previously specified options, use the -option flag
with an empty argument first.






