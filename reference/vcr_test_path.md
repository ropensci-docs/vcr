# Locate file in tests directory

This function, similar to
[`testthat::test_path()`](https://testthat.r-lib.org/reference/test_path.html),
is designed to work both interactively and during tests, locating files
in the `tests/` directory.

## Usage

``` r
vcr_test_path(...)
```

## Arguments

- ...:

  Character vectors giving path components. Each character string gets
  added to the path, e.g., `vcr_test_path("a", "b")` becomes `tests/a/b`
  relative to the root of the package.

## Value

A character vector giving the path

## Note

`vcr_test_path()` assumes you are using testthat for your unit tests.

## Examples

``` r
if (interactive()) {
vcr_test_path("fixtures")
}
```
