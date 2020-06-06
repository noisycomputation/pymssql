.. default-role:: code

.. image:: https://circleci.com/gh/noisycomputation/pymssql-linux/tree/master.svg?style=shield
        :target: https://circleci.com/gh/noisycomputation/pymssql-linux

A simple database interface for `Python`_ that builds on top of `FreeTDS`_ to
provide a Python DB-API (`PEP-249`_) interface to `Microsoft SQL Server`_.

.. _Microsoft SQL Server: http://www.microsoft.com/sqlserver/
.. _Python: http://www.python.org/
.. _PEP-249: http://www.python.org/dev/peps/pep-0249/
.. _FreeTDS: http://www.freetds.org/

The Original Pymssql Project Has Been Discontinued
==================================================

This repository has been forked from upstream pymssql
and modified to work with both Python 3.7 and 3.8,
whilst continuing to build successfully for
2.7 for a short while, for sentimental but quite
fundamental reasons.

There is no interest whatsoever in making this work
in Windows.

The README from the upstream repository is available at
`README_upstream.rst`.

Why Support Python 2 until its dying day?
-----------------------------------------

pymssql is most useful for interfacing with SQL Server 2005,
and if the target environment demands the usage of such old
software, it might also limit one's ability to run Python 3.

It also just felt wrong to drop Python 2 on account of its
deprecation when SQL Server 2005 is so long in the tooth.

Why Not Support Windows?
------------------------

Folks, upstream pymssql was taken out back and shot because
Microsoft released native drivers into the open source. Usage
with SQL Server 2005 requires version 11.0 or earlier
of the MS driver, which is quite tricky to compile and run on a
modern \*nix system due to an entire butterfly network of
dependencies on long-deprecated system libraries. If you've ever
tried to compile something non-trivial on Linux during the
mid-90s, you'll know the pain of which I speak.

If Microsoft had never honored us by bestowing upon us this gift
of deprecated code that hadn't been maintained for a decade,
the upstream pymssql would have been the only game in town and
would have happily chugged along. But Microsoft did honor us,
which led to the behind-the-courthouse death of pymssql, which
led to the earthly hell of trying to compile Microsoft-gathered
\*nix code that relied on libraries from the early 2000s.

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

At a high level, official builds are done using manylinux2010
Docker containers. See the following files for details:

- `.circleci/config.yml`
- `docker-compose.yml`
- `dev/build_manylinux_wheels.sh`

Tests
-----

The testing suite has been inherited from upstream. While
it shows as passing in circleci, it actually fails with
multiple errors, and the successful result in circleci
appears to be the result of the tests being passed into
circleci as shell scripts, whose error-ful exit codes
are not being caught by circleci.

Bringing this project under test will be left to a future
maintainer, hopefully a future maintainer of the upstream
package. This maintainer tested the resulting packages
in the maintainer's use case, and the packages have passed.

Yes, that is anecdotal.

Since the tests do not work, they have been disabled in
`.circleci/config.yml` by commenting out.

