Searching
=========

Search for line in files::
    find ./ -type f -print0 | xargs -0 grep -l "Oh Hai"

Search for iso files, ignoring folders dot folders (i.e. .git, .svn, .ect)::
    find . -type f -not -path '*/\.*' -name *.iso

Search Django::
    BASE_PATH="/home/tom/energysage/env/lib/python2.7/site-packages"
    find "$BASE_PATH/django" -type f -not -path '*/\.*' -name '*.py' -print0 | xargs -0 grep -C 3 -A 2 "Search String"

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
    -z, --compress
        compress file data during the transfer

Copy files from external server
-------------------------------

.. code-block:: bash

    rsync -avz username@server:/home/a/b/target_folders/ target_folder

This will copy the contents from ``target_folder`` on the remote machine into
``target_folder`` on local machine.

Archiving
=========

Create rar archive split across multiple files
----------------------------------------------

.. code-block:: bash
    rar a -v50000 -m0 file.rar thing_to_archive

::
   -m<0..5>
          Set compression level (0-store...3-default...5-best). By default rar uses -m3 method (Normal compression).

   -v<size>[k|b|f]
          Create volumes with size=<size>*1000 [*1024, *1].

tar
---

Create archive::

    tar -pczf name_of_your_archive.tar.gz /path/to/directory

encrypt directory
-----------------

Open Encrypted Directory::

    openssl enc -aes-256-cbc -d -in ~/vault.tar.gz.dat | tar xz; thunar ~/vault

Lock Encrypted Directory::

    tar cz vault/ | openssl enc -aes-256-cbc -out ~/vault.tar.gz.dat; rm -r ~/vault


Screen Capture
==============

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


System Monitoring
=================

disk space (free space)
----------

::
    df -h

Slick One-liners
================

::
    gzip -cd filename.tsv.gz | cut -f2,3 | sort -u | cut -f1 | uniq -c | grep -v " 1 " | cut -c9-  
  

MySQL
=====

Get table column names::
    mysql> describe ext_data_ngusage;
    +--------------+---------------+------+-----+---------+----------------+
    | Field        | Type          | Null | Key | Default | Extra          |
    +--------------+---------------+------+-----+---------+----------------+
    | id           | int(11)       | NO   | PRI | NULL    | auto_increment |
    | value1       | decimal(12,3) | NO   |     | NULL    |                |
    | value2       | decimal(10,2) | NO   |     | NULL    |                |
    | date         | date          | NO   | MUL | NULL    |                |
    | fk_id        | int(11)       | YES  | MUL | NULL    |                |
    +--------------+---------------+------+-----+---------+----------------+
    6 rows in set (0.00 sec)

Find Duplicates
---------------

Get duplicate rows::
    SELECT loc_id, to_date, count(*) AS dupe_count
    FROM ext_data_ngusage
    GROUP BY loc_id, to_date
    HAVING dupe_count > 1;

Write duplicate rows to file::
    mysql -u<mysql user name> -h <host name> -p -e "SELECT loc_id, to_date, count(*) AS dupe_count FROM ext_data_ngusage GROUP BY loc_id, to_date HAVING dupe_count > 1;" <name of database> | cut -f1 | uniq > dup_ids.txt

Systems Programming/Bash
========================

resources
---------

http://shelldorado.com/
`BASH Frequently Asked Questions <http://mywiki.wooledge.org/BashFAQ>`_

streams
-------

streams are referred to by numbers, called file descriptors (FDs)
0 = stdin
1 = stdout
2 = stderr

This sends "OK?" to FILE and "Oops!" to ERRORFILE

.. code-block:: bash

    printf '%s\n%v\n' OK? Oops! > FILE 2> ERRORFILE

redirected to another I/O stream by using >&N where N is the
number of the file descriptor.

.. code-block:: bash

    printf '%s\n%v\n' OK? Oops! > FILE 2>&1 ERRORFILE


SSH
===

Local port listens to 

.. code-block:: bash

    > ssh -L <port>:localhost:<port> <user>@<domain>
    > local_program --port=<port>

Diagrams
========

dot -Tsvg -o callgraph.svg
