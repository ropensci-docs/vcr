# Delete cassettes by prefix

Delete cassettes that match a given prefix from the specified directory
(tests, examples, or vignettes). This is useful for cleaning up old or
unwanted cassettes in batch.

## Usage

``` r
delete_cassettes(prefix, type = c("tests", "examples", "vignettes"))
```

## Arguments

- prefix:

  (character) The prefix to match cassette names. This will match
  cassette names that start with this string. For example,
  `prefix = "api"` will match cassettes like `"api-get.yml"`,
  `"api-post.yml"`, etc. To match a single cassette exactly, include the
  full name without the extension (e.g., `prefix = "my-cassette"` will
  match `"my-cassette.yml"` and `"my-cassette-2.yml"`).

- type:

  (character) The type(s) of cassettes to delete. Can be one or more of:

  - **tests** (default): Delete cassettes from the directory configured
    with
    [`vcr_configure()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md).
    If not configured, defaults to `tests/testthat/_vcr/`.

  - **examples**: Delete cassettes in `inst/_vcr/`

  - **vignettes**: Delete cassettes in `vignettes/_vcr/`

## Value

A character vector of the deleted cassette paths (invisibly). If no
cassettes match the prefix, returns `character(0)`.

## Details

The function will:

1.  Look for cassettes (`.yml`, `.yaml`, `.json`, or `.qs2` extensions)
    in the specified directory or directories.

2.  Match cassettes whose names start with the given prefix.

3.  Delete all matching cassettes.

4.  Report how many cassettes were deleted.

## See also

[`cassette_path()`](https://docs.ropensci.org/vcr/reference/cassettes.md)
for locating cassettes,
[`use_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
for creating cassettes.

## Examples

``` r
if (FALSE) { # \dontrun{
# Delete all test cassettes starting with "api-"
delete_cassettes("api-", type = "tests")

# Delete all example cassettes starting with "github"
delete_cassettes("github", type = "examples")

# Delete a specific vignette cassette (and any cassettes starting with that name)
delete_cassettes("intro-example", type = "vignettes")

# Delete cassettes from multiple locations
delete_cassettes("old-", type = c("tests", "examples"))
} # }
```
