DeclareDesign: Declare and diagnose research designs to understand and
improve them
================

<!-- README.md is generated from README.Rmd. Please edit that file -->

[![Travis-CI Build
Status](https://travis-ci.org/DeclareDesign/DeclareDesign.svg?branch=master)](https://travis-ci.org/DeclareDesign/DeclareDesign)
[![AppVeyor Build
Status](https://ci.appveyor.com/api/projects/status/github/DeclareDesign/DeclareDesign?branch=master&svg=true)](https://ci.appveyor.com/project/DeclareDesign/DeclareDesign)
[![Coverage
Status](https://coveralls.io/repos/github/DeclareDesign/DeclareDesign/badge.svg?branch=master)](https://coveralls.io/github/DeclareDesign/DeclareDesign?branch=master)

## Overview

DeclareDesign is a system for describing research designs in code and
simulating them in order to understand their properties. Because
DeclareDesign relies a consistent grammar of designs, you can focus on
the intellectually challenging part – designing good research studies –
without having to code up simulations from scratch.

## Installation

To install the latest development release of all of the packages, please
ensure that you are running version 3.4 or later of R and run the
following code:

``` r
install.packages("DeclareDesign", dependencies = TRUE,
                 repos = c("http://R.declaredesign.org", "https://cloud.r-project.org"))
```

## Usage

Designs are declared by adding together design elements. Here’s a
minimal example that describes a 100 unit randomized controlled trial
with a binary outcome. Half the units are assigned to treatment and the
remainder to control. The true value of the average treatment effect is
0.05 and it will be estimated with the difference-in-means estimator.
The diagnosis shows that the study is unbiased but underpowered.

``` r
library(DeclareDesign)

design <-
  declare_population(N = 100) +
  declare_potential_outcomes(Y ~ rbinom(n = N, size = 1, prob = 0.5 + 0.05 * Z)) +
  declare_estimand(ATE = 0.05) +
  declare_assignment(m = 50) +
  declare_estimator(Y ~ Z)

diagnosis <- diagnose_design(design, diagnosands = declare_diagnosands(select = c("power", "bias")))
diagnosis
```

    ## 
    ## Research design diagnosis based on 500 simulations. Diagnosand estimates with bootstrapped standard errors in parentheses (100 replicates).
    ## 
    ##  Design Label Estimand Label Estimator Label Coefficient N Sims  Power
    ##        design            ATE       estimator           Z    500   0.10
    ##                                                                 (0.01)
    ##    Bias
    ##   -0.01
    ##  (0.00)

## Philosophy

You can read a short post on the the idea behind DeclareDesign
[here](https://declaredesign.org/idea/). A fuller description of the
philosophy underlying the software is described in this [working
paper](https://declaredesign.org/paper.pdf).

## Companion software

The core DeclareDesign package relies on three companion packages, each
of which is useful in its own right.

1.  [randomizr](https://declaredesign.org/R/randomizr/): Easy to use
    tools for common forms of random assignment and sampling.
2.  [fabricatr](https://declaredesign.org/R/fabricatr/): Imagine your
    data before you collect it.
3.  [estimatr](https://declaredesign.org/R/estimatr/): Fast estimators
    for social scientists.

## Learning DeclareDesigns

To get underway with your own designs, check out our guide on [getting
started with
DeclareDesign](https://declaredesign.org/R/DeclareDesign/articles/DeclareDesign.html),
which covers the main functionality of the software.

You can also browse a [library](https://declaredesign.org/library/) of
already declared designs. The library includes canonical designs that
you can download, modify, and deploy.

-----

This project is generously supported by a grant from the [Laura and John
Arnold Foundation](http://www.arnoldfoundation.org) and seed funding
from [EGAP](http://egap.org).
