# cassette

This vignette tests that we can replay cassettes created by
[`vcr::setup_knitr()`](https://docs.ropensci.org/vcr/reference/setup_knitr.md).

``` r

req <- request(httpbin$url("/get"))
req_perform(req)
#> <httr2_response>
#> GET http://127.0.0.1:45343/get
#> Status: 200 OK
#> Content-Type: application/json
#> Body: In memory (270 bytes)
```

``` r

req <- request(httpbin$url("/get?x=1"))
req_perform(req)
#> <httr2_response>
#> GET http://127.0.0.1:45343/get?x=1
#> Status: 200 OK
#> Content-Type: application/json
#> Body: In memory (286 bytes)
```
