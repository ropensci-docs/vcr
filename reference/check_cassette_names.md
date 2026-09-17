# Check cassette names

**\[deprecated\]**

This function has been deprecated because it's not possible for it to
detect re-used cassette names 100% correctly and the problem of
duplicated cassette names is relatively easy to debug by hand.

## Usage

``` r
check_cassette_names(
  pattern = "test-",
  behavior = "stop",
  allowed_duplicates = NULL
)
```

## Arguments

- pattern:

  (character) regex pattern for file paths to check. This is done inside
  of `tests/testthat/`. Default: "test-".

- behavior:

  (character) "stop" (default) or "warning". If "warning", we use
  `immediate.=TRUE` so the warning happens at the top of your tests
  rather than you seeing it after tests have run (as would happen by
  default).

- allowed_duplicates:

  (character) Cassette names that can be duplicated.
