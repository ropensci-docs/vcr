# Turn vcr on and off

- `turn_on()` and `turn_off()` turn on and off for the whole session.

- `turned_off(code)` temporarily turns off while `code` is running,
  guaranteeing that you make a real HTTP request.

- `turned_on()` reports on if vcr is turned on or not.

- `skip_if_vcr_off()` skips a test if vcr is turned off. This is
  occasionally useful if you're using a cassette to simulate a faked
  request, or if the real request would return different values (e.g.
  you're testing date parsing and the request returns the current date).

You can also control the default behaviour in a new session by setting
the following environment variables before R starts:

- Use `VCR_TURN_OFF=true` to suppress all vcr usage, ignoring all
  cassettes. This is useful for CI/CD workflows where you want to ensure
  the test suite is run against the live API.

- Set `VCR_TURNED_OFF=true` to turn off vcr, but still use cassettes.

## Usage

``` r
turn_on()

turn_off(ignore_cassettes = FALSE)

turned_off(code, ignore_cassettes = FALSE)

turned_on()

skip_if_vcr_off()
```

## Arguments

- ignore_cassettes:

  (logical) Controls what happens when a cassette is inserted while vcr
  is turned off. If `TRUE` is passed, the cassette insertion will be
  ignored; otherwise an error will be raised. Default: `FALSE`

- code:

  Any block of code to run, presumably an HTTP request.

## Examples

``` r
# By default, vcr is turned on
turned_on()
#> [1] TRUE

# you can turn off for the rest of the session
turn_off()
#> vcr turned off; see ?turn_on to turn vcr back on
turned_on()
#> [1] FALSE
# turn on again
turn_on()

# or just turn it on turn off temporarily
turned_off({
  # some HTTP requests here
  turned_on()
})
#> [1] FALSE
```
