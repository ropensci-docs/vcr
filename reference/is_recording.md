# Determine if vcr is recording/replaying

[`local_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
and
[`use_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
set the `VCR_IS_RECORDING` and `VCR_IS_REPLAYING` environment variables
to make it easy to determine vcr state without taking a dependency on
vcr. These functions show you how to use them; we expect you to copy and
paste these functions into your own package

## Usage

``` r
is_recording()

is_replaying()
```

## Value

`TRUE` or `FALSE`.
