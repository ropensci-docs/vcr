# Global Configuration Options

Configurable options that define vcr's default behavior.

## Usage

``` r
vcr_configure(
  dir,
  record,
  match_requests_on,
  serialize_with,
  json_pretty,
  ignore_hosts,
  ignore_localhost,
  preserve_exact_body_bytes,
  turned_off,
  re_record_interval,
  log,
  log_opts,
  filter_sensitive_data,
  filter_sensitive_data_regex,
  filter_request_headers,
  filter_response_headers,
  filter_query_parameters,
  write_disk_path,
  warn_on_empty_cassette
)

local_vcr_configure(..., .frame = parent.frame())

vcr_configure_reset()

vcr_configuration()

vcr_config_defaults()
```

## Arguments

- dir:

  Directory where cassettes are stored.

- record:

  Record mode that dictates how HTTP requests/responses are recorded.
  Possible values are:

  - **once**, the default: Replays recorded interactions, records new
    ones if no cassette exists, and errors on new requests if cassette
    exists.

  - **none**: Replays recorded interactions, and errors on any new
    requests. Guarantees that no HTTP requests occur.

  - **new_episodes**: Replays recorded interactions and always records
    new ones, even if similar interactions exist.

  - **all**: Never replays recorded interactions, always recording new.
    Useful for re-recording outdated responses or logging all HTTP
    requests.

- match_requests_on:

  Character vector of request matchers used to determine which recorded
  HTTP interaction to replay. The default matches on the `"method"`,
  `"uri"`, and either `"body"` (if present) or `"body_json"` (if the
  content-type is `application/json`).

  The full set of possible values are:

  - `method`: the HTTP method.

  - `uri`: the complete request URI, excluding the port.

  - `uri_with_port`: the complete request URI, including the port.

  - `host`: the **host** component of the URI.

  - `path`: the **path** component of the URI.

  - `query`: the **query** component of the URI.

  - `body`: the request body.

  - `body_json`: the request body, parsed as JSON.

  - `header`: all request headers.

  If more than one is specified, all components must match in order for
  the request to match. If not supplied, defaults to
  `c("method", "uri")`.

  Note that the request header and body will only be included in the
  cassette if `match_requests_on` includes "header" or "body"
  respectively. This keeps the recorded request as lightweight as
  possible.

- serialize_with:

  (string) Which serializer to use: `"yaml"` (the default), `"json"`, or
  `"qs2"`.

- json_pretty:

  (logical) want JSON to be newline separated to be easier to read? Or
  remove newlines to save disk space? default: `FALSE`.

- ignore_hosts:

  (character) Vector of hosts to ignore. e.g., `"localhost"`, or
  `"google.com"`. These hosts are ignored and real HTTP requests are
  allowed to go through.

- ignore_localhost:

  (logical) Default: `FALSE`

- preserve_exact_body_bytes:

  (logical) Force a binary (base64) representation of the request and
  response bodies? By default, vcr will look at the `Content-Type`
  header to determine if this is necessary, but if it doesn't work you
  can set `preserve_exact_body_bytes = TRUE` to force it.

- turned_off:

  (logical) VCR is turned on by default. Default: `FALSE`.

- re_record_interval:

  (integer) How frequently (in seconds) the cassette should be
  re-recorded. Default: `NULL` (not re-recorded).

- log, log_opts:

  See
  [`vcr_configure_log()`](https://docs.ropensci.org/vcr/reference/vcr_configure_log.md).

- filter_sensitive_data, filter_sensitive_data_regex:

  Transform header and/or body in the request and response. Named list
  of substitutions to apply to the headers and body of the request and
  response. Format is `list(replacement = "original")` where
  `replacement` is a string that is matched exactly for
  `filter_sensitive_data` and a regular expression for
  `filter_sensitive_data_regex`.

- filter_request_headers, filter_response_headers:

  Filter request or response headers. Should be a list: unnamed
  components are removed, and named components are transformed. For
  example, `list("api_key")` would remove the `api_key` header and
  `list(api_key = "***")` would replace the `api_key` header with `***`.

  httr2's redacted headers are automatically removed.

- filter_query_parameters:

  Filter query parameters in the request. A list where unnamed
  components are removed, and named components are transformed. For
  example, `list("api_key")` would remove the `api_key` parameter and
  `list(api_key = "***")` would replace the `api_key` parameter with
  `***`.

- write_disk_path:

  (character) path to write files to for any requests that write
  responses to disk. By default this will be `{cassette-name}-files/`
  inside the cassette directory.

- warn_on_empty_cassette:

  (logical) Should a warning be thrown when an empty cassette is
  detected? Empty cassettes are cleaned up (deleted) either way. This
  option only determines whether a warning is thrown or not. Default:
  `TRUE`

- ...:

  Configuration settings used to override defaults.

- .frame:

  Attach exit handlers to this environment. Typically, this should be
  either the current environment or a parent frame (accessed through
  [`parent.frame()`](https://rdrr.io/r/base/sys.parent.html)). See
  [`vignette("withr", package = "withr")`](https://withr.r-lib.org/articles/withr.html)
  for more details.

## Examples

``` r
vcr_configure(dir = tempdir())
vcr_configure(dir = tempdir(), record = "all")
vcr_configuration()
#> $dir
#> [1] "/tmp/RtmpDjoVZJ"
#> 
#> $record
#> [1] "all"
#> 
#> $match_requests_on
#> [1] "default"
#> 
#> $serialize_with
#> [1] "yaml"
#> 
#> $json_pretty
#> [1] FALSE
#> 
#> $ignore_hosts
#> NULL
#> 
#> $ignore_localhost
#> [1] FALSE
#> 
#> $preserve_exact_body_bytes
#> [1] FALSE
#> 
#> $turned_off
#> [1] FALSE
#> 
#> $re_record_interval
#> NULL
#> 
#> $log
#> [1] FALSE
#> 
#> $log_opts
#> $log_opts$file
#> [1] "vcr.log"
#> 
#> $log_opts$log_prefix
#> [1] "Cassette"
#> 
#> $log_opts$date
#> [1] TRUE
#> 
#> 
#> $filter_sensitive_data
#> NULL
#> 
#> $filter_sensitive_data_regex
#> NULL
#> 
#> $filter_request_headers
#> NULL
#> 
#> $filter_response_headers
#> NULL
#> 
#> $filter_query_parameters
#> NULL
#> 
#> $write_disk_path
#> NULL
#> 
#> $warn_on_empty_cassette
#> [1] TRUE
#> 
vcr_config_defaults()
#> $dir
#> NULL
#> 
#> $record
#> [1] "once"
#> 
#> $match_requests_on
#> [1] "default"
#> 
#> $serialize_with
#> [1] "yaml"
#> 
#> $json_pretty
#> [1] FALSE
#> 
#> $ignore_hosts
#> NULL
#> 
#> $ignore_localhost
#> [1] FALSE
#> 
#> $preserve_exact_body_bytes
#> [1] FALSE
#> 
#> $turned_off
#> [1] FALSE
#> 
#> $re_record_interval
#> NULL
#> 
#> $log
#> [1] FALSE
#> 
#> $log_opts
#> $log_opts$file
#> [1] "vcr.log"
#> 
#> $log_opts$log_prefix
#> [1] "Cassette"
#> 
#> $log_opts$date
#> [1] TRUE
#> 
#> 
#> $filter_sensitive_data
#> NULL
#> 
#> $filter_sensitive_data_regex
#> NULL
#> 
#> $filter_request_headers
#> NULL
#> 
#> $filter_response_headers
#> NULL
#> 
#> $filter_query_parameters
#> NULL
#> 
#> $write_disk_path
#> NULL
#> 
#> $warn_on_empty_cassette
#> [1] TRUE
#> 
vcr_configure(dir = tempdir(), ignore_hosts = "google.com")
vcr_configure(dir = tempdir(), ignore_localhost = TRUE)

# filter sensitive data
vcr_configure(dir = tempdir(),
  filter_sensitive_data = list(foo = "<bar>")
)
vcr_configure(dir = tempdir(),
  filter_sensitive_data = list(foo = "<bar>", hello = "<world>")
)
```
