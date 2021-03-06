=====================
 Notes for Packagers
=====================

If you are maintaining packages of software that uses `pbr`, there are some
features you probably want to be aware of that can make your life easier.
They are exposed by environment variables, so adding them to rules or spec
files should be fairly easy.

Versioning
==========

`pbr`, when run in a git repo, derives the version of a package from the
git tags. When run in a tarball with a proper egg-info dir, it will happily
pull the version from that. So for the most part, the packager shouldn't need
to care. However, if you are doing something like keeping a git repo with
the sources and the packaging intermixed and it's causing pbr to get confused
about whether its in its own git repo or not, you can set `PBR_VERSION`:

::

  PBR_VERSION=1.2.3

and all version calculation logic will be completely skipped and the supplied
version will be considered absolute.

Dependencies
============

`pbr` overrides almost everything having to do with python dependency
resolution and calls out to `pip`. In the python source package world this
leads to a more consistent experience. However, in the distro packaging world,
dependencies are handled by the distro. Setting `SKIP_PIP_INSTALL`:

::

  SKIP_PIP_INSTALL=1

will cause all logic around use of `pip` to be skipped, including the logic
that includes pip as a dependency of `pbr` itself.

Tarballs
========

`pbr` includes everything in a source tarball that is in the original `git`
repository. This can again cause havoc if a packager is doing fancy things
with combined `git` repos, and is generating a source tarball using `python
setup.py sdist` from that repo. If that is the workflow the packager is using,
setting `SKIP_GIT_SDIST`:

::

  SKIP_GIT_SDIST=1

will cause all logic around using git to find the files that should be in the
source tarball to be skipped. Beware though, that because `pbr` packages
automatically find all of the files, most of them do not have a complete
`MANIFEST.in` file, so its possible that a tarball produced in that way will
be missing files.
