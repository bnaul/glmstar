# Vendored glmnetpp

`include/` here is a verbatim copy of `src/glmnetpp/include` from the CRAN
source tarball of the R package **glmnet 5.0**
(<https://cran.r-project.org/src/contrib/glmnet_5.0.tar.gz>), which is
licensed GPL-2. It is not modified.

On `main` this directory is a git submodule pointing at
`git@github.com:intro-stat-learning/glmnetpp.git`. That repository is private,
so a `pip`/`uv` install from a git URL cannot build the extension modules —
the submodule clone fails for anyone without access. This branch vendors the
headers instead so that

    uv add "glmstar @ git+https://github.com/bnaul/glmstar@release-gil-vendored"

builds anywhere. `eigen` is left as a submodule, since GitLab's Eigen
repository is public and both pip and uv initialize it fine.

The headers compile against `src/*.cpp` unmodified: the
`ElnetDriver::fit(..., setpb_f, int_param)` signature matches what the
pybind11 wrappers call, and CRAN's own `src/internal.h` is a superset of this
repository's. `meson.build` drops `src/glmnetpp/src` and
`src/glmnetpp/test` from the include path, since the CRAN tree has no such
directories (and git cannot track empty ones).

To refresh, or to move to a different glmnet release:

    curl -sLO https://cran.r-project.org/src/contrib/glmnet_<version>.tar.gz
    tar xzf glmnet_<version>.tar.gz
    rm -rf src/glmnetpp/include
    cp -R glmnet/src/glmnetpp/include src/glmnetpp/

This branch exists to make the GIL-release change installable from other
projects. Prefer the submodule on `main` once that change lands upstream.
