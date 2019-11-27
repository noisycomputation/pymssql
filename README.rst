.. default-role:: code

The Pymssql Project is Being Discontinued
==========================================

To install the last working released version, install with a version specifier like "pymssql<3.0".
E.g. `pip install "pymssql<3.0"`

Details and alternatives at: https://github.com/pymssql/pymssql/issues/668

pymssql - DB-API interface to Microsoft SQL Server
==================================================

A simple database interface for `Python`_ that builds on top of `FreeTDS`_ to
provide a Python DB-API (`PEP-249`_) interface to `Microsoft SQL Server`_.

.. _Microsoft SQL Server: http://www.microsoft.com/sqlserver/
.. _Python: http://www.python.org/
.. _PEP-249: http://www.python.org/dev/peps/pep-0249/
.. _FreeTDS: http://www.freetds.org/

There is a Google Group for discussion at:

https://groups.google.com/forum/?fromgroups#!forum/pymssql

Building
========

To build manylinux Python wheels, ensure you have docker and docker-compose
installed, and run the following in the project root directory:

.. code::
    docker-compose up -d
    docker exec pymssql-linux_x86_x64_1 ./io/dev/build_manylinux_wheels.sh
    docker exec pymssql-linux_i686_1 ./io/dev/build_manylinux_wheels.sh
    docker-compose down

To run unit tests, run the following before bringing the containers down:

.. code::
    docker exec pymssql-linux_x86_x64_1 ./io/dev/test_manylinux_wheels.sh
    docker exec pymssql-linux_i686_1 ./io/dev/test_manylinux_wheels.sh
