# Setup vcr for a package

**\[deprecated\]** `use_vcr()` is deprecated because it is no longer
needed. In lifecycle v2,
[`local_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
and friends automatically use `tests/testthat/_vcr` without any need for
additional configuration.

## Usage

``` r
use_vcr(dir = ".", verbose = TRUE)
```

## Arguments

- dir:

  (character) path to package root. default's to current directory

- verbose:

  (logical) print progress messages. default: `TRUE`

## Value

only messages about progress, returns invisible()

## Details

Sets a minimum vcr version, which is usually the latest (stable) version
on CRAN. You can of course easily remove or change the version
requirement yourself after running this function.
