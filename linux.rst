
Search for line in files::
    find ./ -type f -print0 | xargs -0 grep -l "Oh Hai"

xrectsel_ gets x/y offset and size of
rectangular selection::

    xrectsel

.. _xrectsel: https://github.com/lolilolicon/xrectsel


Rsync
-----

Trailing slash on a directory avoids creating an additional directory level at
the destination

The following are the same::

    rsync -a /src/foo /dest
    rsync -a /src/foo/ /dest/foo


openssl
-------

Open Encrypted Directory::

    openssl enc -aes-256-cbc -d -in ~/vault.tar.gz.dat | tar xz; thunar ~/vault

Lock Encrypted Directory::

    tar cz vault/ | openssl enc -aes-256-cbc -out ~/vault.tar.gz.dat; rm -r ~/vault

tar
---

Create archive::

    tar -pczf name_of_your_archive.tar.gz /path/to/directory

