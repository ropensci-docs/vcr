# List cassettes, get current cassette, etc.

List cassettes, get current cassette, etc.

## Usage

``` r
cassettes()

current_cassette()

current_cassette_recording()

current_cassette_replaying()

cassette_path()
```

## Details

- `cassettes()`: returns all active cassettes in the current session.

- `current_cassette()`: returns `NULL` when no cassettes are in use;
  returns the current cassette (a `Cassette` object) when one is in use

- `currrent_cassette_recording()` and `current_cassette_replaying()`:
  tell you if the current cassette is recording and/or replaying. They
  both return `FALSE` if there is no cassette in use.

- `cassette_path()`: returns the current directory path where cassettes
  will be stored

## Examples

``` r
vcr_configure(dir = tempdir())

# list all cassettes
cassettes()
#> list()

# list the currently active cassette
insert_cassette("stuffthings")
current_cassette()
#> <vcr - Cassette> stuffthings
#>   Record method: once
#>   Serialize with: yaml
#>   preserve_exact_body_bytes: FALSE
cassettes()
#> [[1]]
#> <vcr - Cassette> stuffthings
#>   Record method: once
#>   Serialize with: yaml
#>   preserve_exact_body_bytes: FALSE
#> 

eject_cassette()
#> Warning: ✖ "stuffthings" cassette ejected without recording any interactions.
#> ℹ Did you use {curl}, `download.file()`, or other unsupported tool?
#> ℹ If you are using crul/httr/httr2, are you sure you made an HTTP request?
cassettes()
#> list()


# list the path to cassettes
cassette_path()
#> [1] "/tmp/RtmpDjoVZJ"
vcr_configure(dir = file.path(tempdir(), "foo"))
cassette_path()
#> [1] "/tmp/RtmpDjoVZJ/foo"

vcr_configure_reset()
```
