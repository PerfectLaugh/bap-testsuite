BAP Test suite
==============

This repository contains the test artifacts of Binary Analysis Platform,
the binaries, object files, kernel modules and libraries on which the
functional tests of `bap` and `bap-mc` run.

Usage
=====

The tests are OCaml code in the `tools/testsuite` folder of the bap
repository, which checks out this repository in `testsuite/` at the
revision written in `tools/testsuite/REVISION`. Run them in the bap
repository with

```
dune build @testsuite
```

or only the cases whose names contain one of the words of `BAP_TESTS`,
e.g.,

```
BAP_TESTS="taint mc/" dune build @testsuite
```

Changing the tests
------------------

A change of an artifact, or of an output that a test expects, is
committed here, and then the revision in `tools/testsuite/REVISION` of
the bap repository is updated, along with the tests that use it.

The tests of `bap-veri` (the `veri` folder, with `bap-veri` as a git
submodule) still use the DejaGNU framework: `veri.tests` contains the
tests, `lib` the shared code, `site.exp` and `config` the
configuration. Run them with `make veri`, which needs `dejagnu` and
`bap-veri`.

Adding new test artifact
------------------------

We store artifacts in binary mode and if possible in source
code. There are few reasons to commit the artifacts in the binary
form:

1. it is quite to compile them in a portable way
2. sometimes there're now source code for an artifact

The artifact name should have form `tripple-name`, where `tripple` is
a canonical target representation, e.g., `arm-linux-gnueabi`,
`i686-w64-mingw32`, etc. An example of a well-formed fully tripplified
name would be: `arm-linux-androideabi-echo`.

Once a new test artifact is added, make sure that all existing tests
are passing. If the don't then either create an issue (if you think,
that they should pass), or fix the test.

Structure
=========

The `src` folder contains source code for test artifacts. Each
test-artifact that has a source should be rebuildable from the source,
using a recipe in `src/Makefile`. The `src/Makefile` can contain
recipes that are not portable.

The `bin` folder contains the test artifacts built from sources `src`,
or obtained from some obscure system. The folder must contain only
binary files for testing. If some object data is needed, then it
should be stored in corresponding test folder in the test itself or as
a file with `.dat` extension (or any other suitable extension). If you
want to add some meta information to a file that is not backed by a
source code, it would be a good idea to add a textual file with the
same basename as the file into the `src` folder.

The other folders contain the artifacts of particular tests: `dyn` the
dynamic libraries, `obj` the object files, `ko` the kernel modules,
`sse` the floating-point program, `data` the symbols of the
artifacts, and `api` the API of PowerPC.
