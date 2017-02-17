# How to contribute

We'd love to accept your patches and contributions to this project. There are
just a few small guidelines you need to follow.

## Contributor License Agreement

Contributions to this project must be accompanied by a Contributor License
Agreement. You (or your employer) retain the copyright to your contribution,
this simply gives us permission to use and redistribute your contributions as
part of the project. Head over to <https://cla.developers.google.com/> to see
your current agreements on file or to sign a new one.

You generally only need to submit a CLA once, so if you've already submitted one
(even if it was for a different project), you probably don't need to do it
again.

## Code reviews

All submissions, including submissions by project members, require review. We
use GitHub pull requests for this purpose. Consult [GitHub Help] for more
information on using pull requests.

[GitHub Help]: https://help.github.com/articles/about-pull-requests/

## Testing

Test suites are paired with source files at `src/*_test.c` and become
executables named `src/*_test`.

These suites are then accessed by the `*.test` files located at `src/*.test`,
which are simple bash scripts that are used to check environment (such as if you
are root) and pass options to the test suite.

When developing, test with:

```shell
make check
```

Which will run all non-root tests. Optionally, after `make check`, run
`run-root-tests.test` as root to perform tests that require root permissions.

Before sending a pull request, please run the tests again with valgrind:

```shell
make check
cd src
for FILENAME in ./*_test; do
  echo "Running $NAME"
  G_DEBUG=gc-friendly G_SLICE=always-malloc valgrind \
      --leak-check=full --show-possibly-lost=yes --track-origins=yes \
      --leak-resolution=high --num-callers=40 --quiet $FILENAME \
      && echo PASS || echo FAIL
  echo
  echo
done
```

When making a release tarball, use:

```shell
make distcheck
```