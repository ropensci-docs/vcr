# Getting started with vcr

{vcr} records and replays HTTP requests so you can test your API package
with speed and confidence. It makes your tests independent of your
internet connection. That makes your tests faster and more reliable, so
you write even more, increasing the coverage of your package. vcr is
particularly important if your package is on CRAN, because CRAN’s
stringent requirements for package reproducibility are hard to satisfy
when you’re at the mercy of a server on the internet. Using vcr ensures
your tests return the same results regardless of when and where they are
run, letting you submit to CRAN with confidence.

This vignette will introduce you to the basics of vcr. While vcr works
with {crul}, {httr}, and {httr2}, in this vignette, we’ll focus on using
httr2 to generate HTTP requests. The same principles apply if you’re
working with crul or httr, you’ll just use different code for making the
requests. I’m also going to use {webfakes} to run a local web version of
<https://hb.cran.dev>. This lets us make real HTTP requests without
having to worry about whether or not the internet is working.

``` r

library(vcr)
library(testthat)
library(httr2, warn.conflicts = FALSE)

httpbin <- webfakes::local_app_process(webfakes::httpbin_app())
```

## Testing with vcr

The central metaphor of vcr is a video cassette/video tape. This is what
records your HTTP **interactions**, the pairs of HTTP request and
response. You **insert** a cassette to start the recording or begin the
replay, and **eject** it when you’re done. To use vcr in a test, include
a call to
[`vcr::local_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md):
it inserts the cassette and automatically ejects it when the test is
done.

``` r

test_that("my test", {
  vcr::local_cassette("get")

  req <- request(httpbin$url("/get"))
  resp <- req_perform(req)
  json <- resp_body_json(resp)

  expect_named(json, c("args", "headers", "origin", "path", "url"))
})
#> Test passed with 1 success 🥇.
```

The first time this test runs, vcr will record the requests and their
responses in a **cassette**, a YAML file that lives in
`test/testthat/_vcr`. In subsequent runs, vcr will replay those cached
responses, freeing your test from the vagaries of the internet, making
it both faster and more reliable. You might notice that this makes vcr a
bit like a web cache (in that it replays previously saved responses) and
a bit like [snapshot
testing](https://testthat.r-lib.org/articles/snapshotting.html) (in that
we’re storing human-readable data in the test directory).

Get in the habit of running tests twice that use {vcr}, once to record
new cassettes, and a second time immediately after to replay cassettes
to make sure your tests work with recorded cassettes.

Generally, you will want to ensure that every test has a uniquely named
cassette, unless you are deliberately re-using the same request-response
pair to test different parts of your package code. If you try to use the
same cassette for different requests, you’ll discover that it errors
because once a cassette has been recorded, it ensures that httr2, httr,
and crul can only generate responses from the cached interactions.

You can see exactly what vcr is doing with
[`local_vcr_configure_log()`](https://docs.ropensci.org/vcr/reference/vcr_configure_log.md).
This turns logging on so you can see each step in the recording and
replaying process. In this case, since it’s the second run of the test,
you can see that vcr loads the previously saved interaction then replays
it.

``` r

test_that("my test", {
  vcr::local_vcr_configure_log(file = stdout())
  vcr::local_cassette("get")

  req <- request(httpbin$url("/get"))
  resp <- req_perform(req)
  json <- resp_body_json(resp)

  expect_named(json, c("args", "headers", "origin", "path", "url"))
})
#> [Cassette: get] Inserting 'get.yml' (with 1 interactions)
#> [Cassette: get]   Mode: replaying
#> [Cassette: get] Handling request: GET http://127.0.0.1:46881/get
#> [Cassette: get]   Looking for existing requests using method/uri
#> [Cassette: get]     Request 1: MATCH
#> [Cassette: get]   Replaying response 1
#> [Cassette: get] Ejecting
#> Test passed with 1 success 🎊.
```

You can learn more about logging and how to use it to debug cases where
vcr behaves unexpectedly in
[`vignette("debugging")`](https://docs.ropensci.org/vcr/articles/debugging.md).

## Cassette files

It’s good practice to look at the cassette files that are saved to disk,
for instance before committing them to Git or before merging a PR. This
will help you understand how vcr works by seeing exactly what data it
uses. Here’s the cassette we created above:

    #> ```yaml

    #> http_interactions:
    #> - request:
    #>     method: GET
    #>     uri: http://127.0.0.1:46881/get
    #>   response:
    #>     status: 200
    #>     headers:
    #>       Connection: close
    #>       Date: Tue, 18 Aug 2026 19:16:45 GMT
    #>       Content-Type: application/json
    #>       Content-Length: '279'
    #>       ETag: '"89848674"'
    #>     body:
    #>       string: |-
    #>         {
    #>           "args": {},
    #>           "headers": {
    #>             "Host": "127.0.0.1:46881",
    #>             "User-Agent": "httr2/1.3.0 r-curl/7.1.0 libcurl/8.5.0",
    #>             "Accept": "*/*",
    #>             "Accept-Encoding": "deflate, gzip, br, zstd"
    #>           },
    #>           "origin": "127.0.0.1",
    #>           "path": "/get",
    #>           "url": "http://127.0.0.1:46881/get"
    #>         }
    #>   recorded_at: 2026-08-18 19:16:45
    #> recorded_with: VCR-vcr/2.1.1.91

    #> 
    #> ```

You can see that it records all components of the response, along with
the components of the request used for matching (which defaults to
`method` and `uri`; more on that shortly). The first time you use vcr
with your package, it’s a really good idea to closely look at this file
to make sure that you aren’t accidentally leaking any secrets. Learn the
details in
[`vignette("secrets")`](https://docs.ropensci.org/vcr/articles/secrets.md).

Sometimes it also makes sense to manually edit the cassettes. Manually
editing is typically most useful when you’re testing error handling
code, because it allows you to create responses that would otherwise be
hard to get the API to generate.

## Request matching

By default, vcr looks for matching requests using just the HTTP method
and the URI. If you need to match requests differently you can use the
`match_requests_on` parameter. The most likely reason to change this is
to also match on the request body. You can do this in two ways: with
`"body"` or with `"body_json"`. If your API uses JSON (like most modern
APIs) you should use the `body_json` request matcher: that will parse
the body as JSON so you’ll get more informative messages if a new
request doesn’t match a request saved in a cassette.

Here’s a little example with logging turned on, so you can see exactly
what’s happening:

``` r

# A pretend function that would normally be found somewhere in `R/`
get_data <- function(b = 1) {
  req <- request(httpbin$url("/post"))
  req <- req_body_json(req, list(x = 1, y = list(a = 1, b = b)))
  resp <- req_perform(req)

  resp_body_json(resp)
}

# A pretend test that would normally be found in `test/testthat/`.
test_that("my test", {
  vcr::local_vcr_configure_log(file = stdout())
  vcr::local_cassette(
    "body",
    match_requests_on = c("method", "uri", "body_json")
  )

  data <- get_data(b = 1)
  expect_named(data$json, c("x", "y"))
})
#> [Cassette: body] Inserting 'body.yml' (new cassette)
#> [Cassette: body]   Mode: recording
#> [Cassette: body] Handling request: POST http://127.0.0.1:46881/post
#> [Cassette: body]   Recording response: 200 with 518 bytes of application/json data
#> [Cassette: body] Ejecting
#> Test passed with 1 success 🌈.
```

``` r

test_that("my test", {
  vcr::local_vcr_configure_log(file = stdout())
  vcr::local_cassette(
    "body",
    match_requests_on = c("method", "uri", "body_json")
  )

  data <- get_data(b = 2)
  expect_named(data$json, c("x", "y"))
})
#> [Cassette: body] Inserting 'body.yml' (with 1 interactions)
#> [Cassette: body]   Mode: replaying
#> [Cassette: body] Handling request: POST http://127.0.0.1:46881/post
#> [Cassette: body]   Looking for existing requests using method/uri/body_json
#> [Cassette: body]     Request 1: NO MATCH
#> [Cassette: body]       `matching$body$y$b`: 2
#> [Cassette: body]       `recorded$body$y$b`: 1
#> [Cassette: body]   No matching requests
#> [Cassette: body] Ejecting
#> ── Error: my test ──────────────────────────────────────────────────────────────
#> <vcr_unhandled/rlang_error/error/condition>
#> Error in `RequestHandlerHttr2$new(req)$handle()`: Failed to find matching request in active cassette, "body".
#> i Learn more in `vignette(vcr::debugging)`.
#> Backtrace:
#>     ▆
#>  1. └─global get_data(b = 2)
#>  2.   └─httr2::req_perform(req)
#>  3.     └─vcr (local) mock(req)
#>  4.       └─RequestHandlerHttr2$new(req)$handle()
#>  5.         └─cli::cli_abort(...)
#>  6.           └─rlang::abort(...)
#> Error:
#> ! Test failed with 1 failure and 0 successes.
```

If you look at the log you’ll note that we see exactly where the
difference is in the json, even though it’s buried several layers deep.
Compare this to just using `"body"`, where you have to carefully compare
two strings.

``` r

test_that("my test", {
  vcr::local_vcr_configure_log(file = stdout())
  vcr::local_cassette("body", match_requests_on = c("method", "uri", "body"))

  data <- get_data(b = 2)
  expect_named(data$json, c("x", "y"))
})
#> [Cassette: body] Inserting 'body.yml' (with 1 interactions)
#> [Cassette: body]   Mode: replaying
#> [Cassette: body] Handling request: POST http://127.0.0.1:46881/post {"x":1,"y":{"a":1,"b":2}}
#> [Cassette: body]   Looking for existing requests using method/uri/body
#> [Cassette: body]     Request 1: NO MATCH
#> [Cassette: body]       `matching$body`: "{\"x\":1,\"y\":{\"a\":1,\"b\":2}}"
#> [Cassette: body]       `recorded$body`: "{\"x\":1,\"y\":{\"a\":1,\"b\":1}}"
#> [Cassette: body]   No matching requests
#> [Cassette: body] Ejecting
#> ── Error: my test ──────────────────────────────────────────────────────────────
#> <vcr_unhandled/rlang_error/error/condition>
#> Error in `RequestHandlerHttr2$new(req)$handle()`: Failed to find matching request in active cassette, "body".
#> i Learn more in `vignette(vcr::debugging)`.
#> Backtrace:
#>     ▆
#>  1. └─global get_data(b = 2)
#>  2.   └─httr2::req_perform(req)
#>  3.     └─vcr (local) mock(req)
#>  4.       └─RequestHandlerHttr2$new(req)$handle()
#>  5.         └─cli::cli_abort(...)
#>  6.           └─rlang::abort(...)
#> Error:
#> ! Test failed with 1 failure and 0 successes.
```

## What happens if the API changes?

vcr isolates your tests from the internet. Most of the time that’s great
because it keeps your tests fast and protects them against any minor
glitches. But what happens if the API fundamentally changes? Your tests
will continue to pass but real code will fail. This shouldn’t generally
be a problem with well designed APIs, because a well designed API will
include an explicit version either in the URL or in a header. But not
every API is well designed, and you probably want some protection in
either case. There are two main options:

- Include a small number of tests that don’t use cassettes, so that they
  always use the live API. These tests should be protected by
  [`skip_on_cran()`](https://testthat.r-lib.org/reference/skip.html) so
  that they never run on CRAN.

- Set up a GitHub action (or similar) that runs `R CMD check` with env
  var `VCR_TURN_OFF=true`. This turns off all vcr usage so that all
  requests are live. You might want to set this up to run weekly, so you
  find out when the API changes (even when your package doesn’t), but
  aren’t overwhelmed with notifications.

Learn more in
<https://books.ropensci.org/http-testing/real-requests-chapter.html>.

## Other uses: examples and vignettes

Now that you’re familiar with using vcr for your unit tests, you might
wonder if there are other places you can use it in your packages. There
are! Two other important use cases are in examples and vignettes:

- Use
  [`vcr::setup_knitr()`](https://docs.ropensci.org/vcr/reference/setup_knitr.md)
  in your vignettes to enable a special knitr chunk hook that allows you
  to use vcr cassettes by setting the `cassette` chunk option.

- Use
  [`vcr::insert_example_cassette()`](https://docs.ropensci.org/vcr/reference/insert_example_cassette.md)
  and
  [`vcr::eject_cassette()`](https://docs.ropensci.org/vcr/reference/insert_cassette.md)
  in examples. You can surround these commands in `\dontshow{}` so your
  users don’t see them, but they’re still run.

Just as in testing, for both of these use cases immediately replay the
cassettes after first recording any new ones to make sure the replayed
cassettes work.

If you use {pkgdown} to generate package documentation, using it to
build examples and vignettes is a great way to both create cassettes on
a first run of
[`pkgdown::build_site()`](https://pkgdown.r-lib.org/reference/build_site.html)
and make sure they work on a second and any subsequent runs.

Read the documentation for these functions to get all the details.

## Conclusion

vcr offers a powerful solution for testing API-dependent packages,
making your tests faster, more reliable, and CRAN-friendly. By recording
HTTP interactions and replaying them in subsequent test runs, it
isolates your tests from internet connectivity issues and API
fluctuations.

To recap the key points:

- Use
  [`vcr::local_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
  in your tests to record and replay HTTP interactions.
- Examine cassette files to understand what’s being recorded and to
  ensure no secrets are leaked.
- Customize request matching with `match_requests_on` when needed.
- Consider strategies for detecting API changes, like occasional live
  tests.
- Use
  [`vcr::setup_knitr()`](https://docs.ropensci.org/vcr/reference/setup_knitr.md)
  and
  [`vcr::insert_example_cassette()`](https://docs.ropensci.org/vcr/reference/insert_example_cassette.md)
  to also use vcr in examples and vignettes.

For more advanced use cases and troubleshooting, explore the other
vignettes in the package, particularly
[`vignette("debugging")`](https://docs.ropensci.org/vcr/articles/debugging.md)
and
[`vignette("secrets")`](https://docs.ropensci.org/vcr/articles/secrets.md).
The [HTTP testing book](https://books.ropensci.org/http-testing/) is
also an excellent resource for deepening your understanding of HTTP
testing in R.
