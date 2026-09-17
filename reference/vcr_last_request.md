# Retrieve last vcr request/response

When debugging, it's often useful to see the last request and response
respectively. These functions give you what would have been recorded to
disk or replayed from disk. If the last request wasn't recorded, and
there was no matching request, `vcr_last_response` will return `NULL`.

## Usage

``` r
vcr_last_request()

vcr_last_response()
```
