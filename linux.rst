Searching
=========

Search for line in files::
    find ./ -type f -print0 | xargs -0 grep -l "Oh Hai"


Copying
=======

Copy files from ``/foo/bar/`` to ``/backups/bar`` skipping files that already
exist on ``/backups/bar``::

    rsync -avu --ignore-existing "/foo/bar/" "/backups/bar"

Comparing files based on size and time (the default) is faster than using a
checksum.

Basic Options::

    -a archive
    -v verbose
    -c, --checksum
        skip based on checksum, not mod-time & size
    -u, --update
        skip files that are newer on the receiver
    -i, --itemize-changes
        output a change-summary for all updates
    --ignore-existing
        skip updating files that exist on receiver

    -n, --dry-run
        perform a trial run with no changes made


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





