.. default-role:: code

The Original Pymssql Project Has Been Discontinued
==================================================

The last working released version may or may not work with
Python 3.7, but does not with with 3.8. To install it, run
`pip install "pymssql<3.0"`.

This repository has been modified to work with both Python 3.7
and 3.8. To build pymssql-linux, you should have:

* python >= 3.7 including development files.
* Cython >= 0.29
* FreeTDS >= 0.95 including development files. Research your
  OS-specific distribution channels. (Archlinux: freetds,
  Debian: freetds-common, freetds-dev)
* gcc

To build, simply run `python setup.py build` in the project
root directory.

It is possible to build a binary wheel package of pymssql-linux
that pulls in and compiles a known-working version of FreeTDS.
This option may become necessary if FreeTDS code evolves in a
way that breaks compatibility with pymssql-linux, the development
of which is, after all, frozen. Follow the binary wheel build
instructions below in this case.

Details about the discontinuation of the original project
and a discussion of alternatives to pymssql can be found
at: https://github.com/pymssql/pymssql/issues/668

This fork is being maintained because pymssql works with
older SQL Server versions that use deprecated TLS versions
1.0 and 1.1. Alternatives that utilize Microsoft's native
SQL driver require the installation of version 11.0 of the
driver, which is difficult to achieve cleanly due to
multiple dependencies on deprecated library versions.

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

Building Binary Wheels
======================

To build manylinux Python wheels, ensure you have docker and docker-compose
installed, and run the following in the project root directory:

.. code-block:: bash

    docker-compose up -d
    docker exec pymssql-linux_x86_x64_1 ./io/dev/build_manylinux_wheels.sh
    docker exec pymssql-linux_i686_1 ./io/dev/build_manylinux_wheels.sh
    docker-compose down

To run unit tests, run the following before bringing the containers down:

.. code-block:: bash

    docker exec pymssql-linux_x86_x64_1 ./io/dev/test_manylinux_wheels.sh
    docker exec pymssql-linux_i686_1 ./io/dev/test_manylinux_wheels.sh

If the build suceeds, the `dist` directory in the project root will
contain .whl files for Python versions >= 3.7. These can be installed
by running `pip install <filename.whl>`.
