# Tyr Language Conformance Tests

This repository contains tests grouped by release.
The main purpose is to ensure that we do not break something that worked before accidentally.
The secondary goal of this repository is to direct the evolution of tools and the language itself.
As such, the repository contains tests for scheduled releases as well.


## Organization

The smallest submittable test unit is a package.
The package is fed into the compiler as if "test -P" were used.
The path to the package determines the expected outcome.
Hence, the layout of the repository is:

./[version]/[accept/fail]/[test]/[testfiles/**]

version: The name of the release, e.g "0.5.0". Tests added after the first release of a version ought to be located in name-reg, e.g. "0.5.0-reg".

accept/fail: Tests that shall not pass the compiler have to be placed below fail. Other tests shall be placed in accept. An accepted test packag passes if it contains at least one test and all tests pass.

test: The name of the test group.

testfiles: Files belonging to this test.
This includes at least a file package.draupnir.
If a test requires a only a single source file, that file shall be named mar.tyr and it shall begin with a block comment containing a short description of the test's intentions.


## Implications

A compiler conforms with a version of Tyr iff it passes all tests below that version and all prior versions including reg-directories.


## Migration

We will migrate old tests to semantic equivalents if the current release renders them illegal, as long as the major version is 0.
Exact error categories are subject to change for the time being.

This section ought to be deleted on the release of version 1.0.0.
