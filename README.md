
<!-- README.md is generated from README.Rmd. Please edit that file -->

# unifir: A Unifying API for Working with Unity in R <img src='man/figures/logo.png' align="right" height="138" />

<!-- badges: start -->

[![License:
MIT](https://img.shields.io/badge/license-MIT-green)](https://choosealicense.com/licenses/mit/)
[![Project Status: Active – The project has reached a stable, usable
state and is being actively
developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![Lifecycle:
maturing](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://lifecycle.r-lib.org/articles/stages.html#maturing-1)
![CRAN status](https://www.r-pkg.org/badges/version/unifir)
[![R-CMD-check](https://github.com/ropensci/unifir/workflows/R-CMD-check/badge.svg)](https://github.com/ropensci/unifir/actions)
[![Codecov test
coverage](https://codecov.io/gh/ropensci/unifir/branch/main/graph/badge.svg)](https://app.codecov.io/gh/ropensci/unifir?branch=main)
[![Status at rOpenSci Software Peer
Review](https://badges.ropensci.org/521_status.svg)](https://github.com/ropensci/software-review/issues/521)
<!-- badges: end -->

unifir is a unifying API for creating and managing
[Unity](https://unity.com/) scenes directly from R (without requiring
GUI interaction). Users are able to write natural-feeling R code to
create “scripts” (C# programs) composed of “props” (C# methods) which
produce scenes inside the Unity engine. While it is entirely possible to
create fleshed-out Unity scenes through this package, unifir is
primarily designed around being easy to wrap in other packages, so that
any number of packages interacting with Unity can all share a common
framework.

The first function most unifir workflows will call is `make_script`,
which creates a “script” object. In that script, we can tell unifir
things like where we want to save our project:

``` r
library(unifir)
script <- make_script(project = "path/to/project")
```

We can then iteratively add props to our project, building up a series
of commands that we’ll execute in sequence to create a Unity scene:

``` r
script <- add_default_player(script, x_position = 10)
script <- add_light(script)
script <- save_scene(script)
```

All of these functions are designed with the pipe in mind, letting you
easily simplify your code:

``` r
script <- make_script(project = "path/to/project") |> 
  add_default_player(x_position = 10) |> 
  add_light() |> 
  save_scene()
```

Once all your props are set, it’s time to execute the script via the
`action` function! This function will create a new Unity project (if
necessary), write your “script” object to a C# file, and execute it
inside the Unity project.

``` r
action(script)
```

Open your new project from UnityHub, open the scene you created with
`save_scene`, and you’ll see the outputs from your script right in front
of you! To learn more, check out the vignettes that ship with the
package – particularly [the one for how to use unifir to make
scenes](https://docs.ropensci.org/unifir/articles/unifir-user-guide.html)
and [the one for how to extend unifir in your own
packages](https://docs.ropensci.org/unifir/articles/unifir-dev-guide.html).

Right now, unifir wraps the elements of the Unity API that I’ve found
useful in my own work, principally focused around creating GameObjects,
lights, player controllers, and terrain surfaces. If you are interested
in pieces of the API that haven’t made it into unifir yet, please open
an issue or a PR!

## Installation

The easiest way to install unifir is to install it directly from
R-Universe:

``` r
# Enable universe(s) by rOpenSci
options(repos = c(
  mikemahoney218 = 'https://ropensci-universe.dev',
  CRAN = 'https://cloud.r-project.org'))

# Install unifir
install.packages('unifir')
```

You can also install unifir from [GitHub](https://github.com/) with:

``` r
# install.packages("remotes")
remotes::install_github("ropensci/unifir")
```

Note that right now there is no release version of unifir; all published
versions are considered “development” versions. While the unifir API is
solidifying, it is not yet solid, and breaking changes may happen at any
time.

While unifir can be used by itself, you’ll probably want to [install
Unity](https://unity.com/download) in order to actually make Unity
projects.

## Why do this?

There’s been a lot of interest in using immersive virtual environments
for research on topics including [science
communication](https://doi.org/10.1080/13658816.2020.1830997),
[landscape planning](https://doi.org/10.1016/j.apgeog.2019.102102),
[environmental economics](https://doi.org/10.1016/j.jeem.2008.08.002)
and beyond. This is an incredibly exciting area of research, which looks
likely to present new methods for participatory planning, data
visualization, and skill training. However, right now, much of the
research in this area relies upon closed-source tooling which puts up
hurdles in front of the equitability, accessibility, and
interoperability of the field. In addition, a challenge when assessing
hand-build environments (say to assess participant preferences between
two scenes) is that us as researchers might accidentally “tilt the
scales” in favor of our preferred option, by putting a little more
effort into making the scene we personally prefer look more
aesthetically pleasing than the alternative.

unifir is a first step towards an open-source, fully reproducible
approach for constructing immersive virtual environments. While it
currently only targets Unity, a proprietary source-available game
engine, unifir hopes to improve the openness and interoperability of
immersive virtual environments by encoding all of the decisions involved
in building a scene in standard R and C# code. A partially open system
is better than a fully closed one, and if other game engines become more
feasible options for large scale immersive virtual environments, unifir
may be extended to support those as well.

## Code of Conduct

Please note that the unifir project is released with a [Contributor Code
of Conduct](https://ropensci.github.io/unifir/CODE_OF_CONDUCT.html). By
contributing to this project, you agree to abide by its terms.

## Disclaimer

These materials are not sponsored by or affiliated with Unity
Technologies or its affiliates. “Unity” is a trademark or registered
trademark of Unity Technologies or its affiliates in the U.S. and
elsewhere.

[![ropensci_footer](https://ropensci.org/public_images/github_footer.png)](https://ropensci.org)
