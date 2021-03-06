
<!-- README.md is generated from README.Rmd. Please edit that file -->

[![Travis-CI Build
Status](https://travis-ci.org/bodkan/admixr.svg?branch=master)](https://travis-ci.org/bodkan/admixr)
[![Coverage
status](https://codecov.io/gh/bodkan/admixr/branch/master/graph/badge.svg)](https://codecov.io/github/bodkan/admixr?branch=master)
[![lifecycle](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)

# admixr

## Introduction

[ADMIXTOOLS](https://github.com/DReichLab/AdmixTools/) is a widely used
software package for calculating admixture statistics and testing
population admixture hypotheses. However, although powerful and
comprehensive, it is not exactly known for being user-friendly.

A typical ADMIXTOOLS workflow often involves a combination of
`sed`/`awk`/shell scripting and manual editing to create different
configuration files. These are then passed as command-line arguments to
one of ADMIXTOOLS commands, and control how to run a particular
analysis. The results are then redirected to another file, which has to
be parsed by the user to extract values of interest, often using
command-line utilities again or (worse) by manual copy-pasting. Finally,
the processed results are analysed in R, Excel or another program.

This workflow is very cumbersome, especially if one wants to explore
many hypotheses involving different combinations of populations. Most
importantly, however, it makes it difficult to follow the rules of best
practice for reproducible science, as it is nearly impossible to
construct reproducible automated “pipelines”.

This R package makes it possible to perform all stages of an ADMIXTOOLS
analysis entirely from R. It provides a set of convenient functions that
completely remove the need for “low level” configuration of individual
ADMIXTOOLS programs, allowing users to focus on the analysis itself.

## How to cite

*admixr* is now published as an [*Application
Note*](https://doi.org/10.1093/bioinformatics/btz030) in the journal
Bioinformatics. If you use it in your work, please cite the paper\!

## Installation instructions

**Note (2019/04/06):** This package is currently in the last phase of
testing and polishing the API before I submit it to
[CRAN](https://cran.r-project.org). You can definitely safely use it for
your own work, but please make sure to check back here for instructions
on how to install the officially released version. You can also follow
me on [Twitter](https://www.twitter.com/fleventy5) if you want to stay
updated.

To install `admixr` from Github you need to install the package
`devtools` first. You can simply run:

``` r
install.packages("devtools")
devtools::install_github("bodkan/admixr")
```

If you want to **update** *admixr* to a newer version, simply run
`devtools::install_github("bodkan/admixr")` again.

**Note that in order to use the `admixr` package, you need a working
installation of ADMIXTOOLS\!** You can find installation instructions
[here](https://github.com/DReichLab/AdmixTools/blob/master/README.INSTALL).

**Furthermore, you need to make sure that R can find ADMIXTOOLS binaries
on the `$PATH`.** If this is not the case, running `library(admixr)`
will show a warning message with instructions on how to fix this.

## Example

This is all the code that you need to perform ADMIXTOOLS analyses using
this package\! No shell scripting, no copy-pasting and manual editing of
text files. The only thing you need is a working ADMIXTOOLS installation
and a path to EIGENSTRAT data (a trio of ind/snp/geno files), which we
call `prefix` here.

``` r
library(admixr)

# download a small testing dataset to a temporary directory and
# process it for use in R
snp_data <- eigenstrat(download_data())

result <- d(
  W = c("French", "Sardinian"), X = "Yoruba", Y = "Vindija", Z = "Chimp",
  data = snp_data
)

result
#> # A tibble: 2 x 10
#>   W         X      Y       Z          D  stderr Zscore  BABA  ABBA  nsnps
#>   <chr>     <chr>  <chr>   <chr>  <dbl>   <dbl>  <dbl> <dbl> <dbl>  <dbl>
#> 1 French    Yoruba Vindija Chimp 0.0313 0.00693   4.51 15802 14844 487753
#> 2 Sardinian Yoruba Vindija Chimp 0.0287 0.00679   4.22 15729 14852 487646
```

Note that a single call to the `d` function generates all required
intermediate config and population files, runs ADMIXTOOLS, parses its
log output and returns the result as a `data.frame` object. It does all
of this behind the scenes, without the user having to deal with
low-level technical details.

**To see many more examples, please check out the [tutorial
vignette](https://bodkan.net/admixr/articles/tutorial.html).**
