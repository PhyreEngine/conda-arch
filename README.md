This repository contains a [conda][conda] recipe for a metapackage that allows
conda to choose between builds of certain packages intended for different CPU
architectures.

Some software (notably [hhsuite][hh]) will not compile unless certain CPU
instructions are available, and benefit heavily when the compiler is allowed to
optimise aggressively for a particular CPU architecture. It is difficult to
build generic conda packages for software with CPU-specific requirements; even
if it can be done, the resulting binary will likely be quite slow. We sidestep
the problem by building conda packages for several different CPU architectures,
but this then makes choosing the correct package a pain.

This metapackage provides several conda packages that track certain "features".
Basically, if you have a sandybridge CPU, you should install the
`arch=*=sandybrige` metapackage (that is, any version of the `arch` package
with the build string `sandybridge`), which will cause conda to prefer packages
built with the `arch_sandybridge` feature.

## Prerequisites

1. You will need an installation of [conda][miniconda].

2. Your root conda installation must have the `conda-build` installed.

## Building

You should be able to build this package by simply running `conda build .`.
This repository includes `conda_build_config.yaml`, which causes multiple
versions of this metapackage to be built, one for each architecture.

[conda]: https://conda.io
[miniconda]: https://conda.io/miniconda.html
[hh]: https://github.com/soedinglab/hh-suite
