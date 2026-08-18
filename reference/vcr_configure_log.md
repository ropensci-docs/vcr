# Configure vcr logging

By default, logging is disabled, but you can easily enable for the
entire session with `vcr_configure_log()` or for just one test with
`local_vcr_configure_log()`.

## Usage

``` r
vcr_configure_log(
  log = TRUE,
  file = stderr(),
  include_date = NULL,
  log_prefix = "Cassette"
)

local_vcr_configure_log(
  log = TRUE,
  file = stderr(),
  include_date = NULL,
  log_prefix = "Cassette",
  frame = parent.frame()
)
```

## Arguments

- log:

  Should we log important vcr things?

- file:

  A path or connection to log to

- include_date:

  (boolean) Include date and time in each log entry.

- log_prefix:

  "Cassette". We insert the cassette name after this prefix, followed by
  the rest of the message.

- frame:

  Attach exit handlers to this environment. Typically, this should be
  either the current environment or a parent frame (accessed through
  [`parent.frame()`](https://rdrr.io/r/base/sys.parent.html)). See
  [`vignette("withr", package = "withr")`](https://withr.r-lib.org/articles/withr.html)
  for more details.

## Examples

``` r
# The default logs to stderr()
vcr_configure_log()

# But you might want to log to a file
vcr_configure_log(file = file.path(tempdir(), "vcr.log"))
```
