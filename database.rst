PostgreSql
==========

Dropping all tables
-------------------

::

    >> psql -h localhost -p 5432 -U postgres djinvoice
    djinvoice=# drop schema public cascade;
    djinvoice=# create schema public;

MongoDB
==========


