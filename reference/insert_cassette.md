# Manually insert and eject a cassette

Generally you should not need to use these functions, instead preferring
[`use_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
or
[`local_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)

## Usage

``` r
insert_cassette(
  name,
  dir = NULL,
  record = NULL,
  match_requests_on = NULL,
  serialize_with = NULL,
  preserve_exact_body_bytes = NULL,
  re_record_interval = NULL,
  warn_on_empty = NULL
)

eject_cassette()
```

## Arguments

- name:

  The name of the cassette. This is used to name a file on disk, so it
  must be valid file name.

- dir:

  The directory where the cassette will be stored. Defaults to
  `test_path("_vcr")`.

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

- preserve_exact_body_bytes:

  (logical) Force a binary (base64) representation of the request and
  response bodies? By default, vcr will look at the `Content-Type`
  header to determine if this is necessary, but if it doesn't work you
  can set `preserve_exact_body_bytes = TRUE` to force it.

- re_record_interval:

  (integer) How frequently (in seconds) the cassette should be
  re-recorded. Default: `NULL` (not re-recorded).

- warn_on_empty:

  (logical) Warn if the cassette is ejected but no interactions have
  been recorded. Default: `NULL` (inherits from global configuration).

## Value

A [Cassette](https://docs.ropensci.org/vcr/reference/Cassette.md),
invisibly.

## Examples

``` r
vcr_configure(dir = tempdir())

insert_cassette("hello")
current_cassette()
#> <vcr - Cassette> hello
#>   Record method: once
#>   Serialize with: yaml
#>   preserve_exact_body_bytes: FALSE

eject_cassette()
#> Warning: ✖ "hello" cassette ejected without recording any interactions.
#> ℹ Did you use {curl}, `download.file()`, or other unsupported tool?
#> ℹ If you are using crul/httr/httr2, are you sure you made an HTTP request?
current_cassette()
#> NULL
```
