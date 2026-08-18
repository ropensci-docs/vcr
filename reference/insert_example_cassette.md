# Use cassettes in examples

`insert_example_cassette()` is a wrapper around
[`insert_cassette()`](https://docs.ropensci.org/vcr/reference/insert_cassette.md)
that stores cassettes in `inst/_vcr/`. Call it in the first line of your
examples (typically wrapped in `\dontshow{}`), and call
[`eject_cassette()`](https://docs.ropensci.org/vcr/reference/insert_cassette.md)
on the last line.

Run the example manually once to record the vignettte, then it will be
replayed during `R CMD check`, ensuring that your example no longer uses
the internet.

## Usage

``` r
insert_example_cassette(
  name,
  package,
  record = NULL,
  match_requests_on = NULL,
  serialize_with = NULL,
  preserve_exact_body_bytes = NULL,
  re_record_interval = NULL
)
```

## Arguments

- name:

  The name of the cassette. This is used to name a file on disk, so it
  must be valid file name.

- package:

  Package name.

- record:

  Record mode. This will be `"once"` if `package` is under development,
  (i.e. loaded by devtools) and `"none"` otherwise. This makes it easy
  to record during development and ensure that cassettes HTTP requests
  are never made on CRAN.

  To re-record all cassettes, you can delete `inst/_vcr` then run
  `pkgdown::build_reference(lazy = FALSE)`.

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

- preserve_exact_body_bytes:

  (logical) Force a binary (base64) representation of the request and
  response bodies? By default, vcr will look at the `Content-Type`
  header to determine if this is necessary, but if it doesn't work you
  can set `preserve_exact_body_bytes = TRUE` to force it.

- re_record_interval:

  (integer) How frequently (in seconds) the cassette should be
  re-recorded. Default: `NULL` (not re-recorded).

## Examples

``` r
# In this example I'm showing the insert and eject commands, but you'd
# usually wrap these in \dontshow{} so the user doesn't see them and
# think that they're something they need to copy.

insert_example_cassette("httpbin-get", package = "vcr")

req <- httr2::request("https://hb.cran.dev/get")
resp <- httr2::req_perform(req)

str(httr2::resp_body_json(resp))
#> List of 4
#>  $ args   : Named list()
#>  $ headers:List of 10
#>   ..$ Accept          : chr "*/*"
#>   ..$ Accept-Encoding : chr "gzip, br"
#>   ..$ Cdn-Loop        : chr "cloudflare; loops=1"
#>   ..$ Cf-Connecting-Ip: chr "2600:1700:d01:845f:f8e0:f28d:f0a:d1f"
#>   ..$ Cf-Ipcountry    : chr "US"
#>   ..$ Cf-Ray          : chr "93b8ae95e81469de-DFW"
#>   ..$ Cf-Visitor      : chr "{\"scheme\":\"https\"}"
#>   ..$ Connection      : chr "close"
#>   ..$ Host            : chr "httpbin:8080"
#>   ..$ User-Agent      : chr "httr2/1.1.2 r-curl/6.2.2 libcurl/8.11.1"
#>  $ origin : chr "2600:1700:d01:845f:f8e0:f28d:f0a:d1f"
#>  $ url    : chr "https://httpbin:8080/get"

eject_cassette()
```
