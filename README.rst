.. default-role:: code

.. image:: https://circleci.com/gh/noisycomputation/pymssql-linux/tree/master.svg?style=shield
        :target: https://circleci.com/gh/noisycomputation/pymssql-linux

The Original Pymssql Project Has Been Discontinued
==================================================

This repository has been forked from upstream pymssql
and modified to work with both Python 3.7 and 3.8,
whilst continuing to build successfully for
2.7 for a short while, for sentimental but quite
fundamental reasons.

There is no interest whatsoever in making this work
in Windows.

Why Support Python 2 until its dying day?
-----------------------------------------

Folks, upstream pymssql was taken out back and shot because
Microsoft released native drivers into the open source. Using
MS drivers on SQL Server 2005 requires version 11.0 or earlier
of the MS driver, which is nearly impossible to compile on a
modern \*nix system due to an entire butterfly network of
dependencies on long-deprecated system libraries. If you've ever
tried to compile something non-trivial on Linux during the
mid-90s, you'll know the pain of which I speak.

Why Not Support Windows?
------------------------

See above. If Microsoft never honored us by gifting us a bunch
of deprecated code that hadn't been maintained for a decade,
the upstream pymssql would have been the only game in town and
would have happily chugged along. But Microsoft did honor us,
which led to the behind-the-courthouse death of pymssql, which
led to the earthly hell of trying to compile Microsoft-gathered
\*nix code that relied on libraries from the early 2000s, et
cetera, ad infinitum.

I have zero interest in figuring out how to build pymssql in
a Windows CI. If someone wants to work with me to port
pymssql to the Commodore 64, the Amiga 500, or Windows 2.1,
let us waste no time. But forget Windows >= 3.1.

It must be childishly easy to get the official drivers working
on a Windows box anyways.

Build Instructions
------------------

One of the reasons the upstream repository became such a tangle
of inconsistent and outdated information is that it contained
a large amount of build-related cruft that accumulated over
the years. And that cruft was in addition to a README that
contained build instructions that *maybe* worked two versions
ago.

To avoid this fate, this repository will maintain only those
CI configs that are being used to build, test, and deploy
this package to PyPi. Currently, that universe consists of
one CI, *circleci*, chosen only because it was the repository of
the most valid build instructions at the time this repo
was forked from upstream. This may change in the future.

Though this README may provide some additional context,
the only set of build instructions that are guaranteed
to be valid will be contained in the config file(s) for
the current CI system being used. For the branch to which
this README document pertains, the CI system is circleci
and the build instructions are at `.circleci/config.yml`

Though referencing a CI configuration file is not as
user-friendly as writing a soliloquy, it has the benefit
of being verifiably accurate. If the circleci status badge
at the top of this README indicates that the last build
succeeded, then the circleci build instructions are valid.

Tests
-----

The testing suite has been inherited from upstream. While
it shows as passing in circleci, it actually fails with
multiple errors, and the successful result in circleci
appears to be the result of the tests being passed into
circleci as shell scripts, whose error-ful exit codes
are not being passed to circleci.

Bringing this project under test will be left to a future
maintainer, hopefully a future maintainer of the upstream
package. This maintainer tested the resulting packages
in the maintainer's use case, and the packages have passed.

Yes, that is anecdotal.

Since the tests do not work, they have been disabled in
`.circleci/config.yml`.


The remainder of this document is from the upstream
repository and is likely outdated, especially as it pertains
to build instructions. As my kinsfolk say, *szerokiej drogi*.

Original Pymssql Readme
=======================

To build pymssql-linux, you should have:

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
