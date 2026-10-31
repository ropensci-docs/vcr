# vcr: Record 'HTTP' Calls to Disk

Record test suite 'HTTP' requests and replays them during future runs. A
port of the Ruby gem of the same name (<https://github.com/vcr/vcr/>).
Works by recording real 'HTTP' requests/responses on disk in
'cassettes', and then replaying matching responses on subsequent
requests.

## Backstory

A Ruby gem of the same name (`VCR`, <https://github.com/vcr/vcr>) was
created many years ago and is the original. Ports in many languages have
been done. Check out that GitHub repo for all the details on how the
canonical version works.

## Main functions

The
[`use_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
function is most likely what you'll want to use. It sets the cassette
you want to record to, inserts the cassette, and then ejects the
cassette, recording the interactions to the cassette.

Alternatively, you can use
[`insert_cassette()`](https://docs.ropensci.org/vcr/reference/insert_cassette.md)
for more control, but then you have to make sure to use
[`eject_cassette()`](https://docs.ropensci.org/vcr/reference/insert_cassette.md).

## vcr configuration

[`vcr_configure()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md)
is the function to use to set R session-wide settings. See its manual
file for help.

## Async

As of crul v1.5, `vcr` will work for async http requests with crul. httr
does not do async requests, and httr2 async plumbing does not have any
hooks for mocking via webmockr or recording real requests via this
package.

## See also

Useful links:

- <https://github.com/ropensci/vcr/>

- <https://books.ropensci.org/http-testing/>

- <https://docs.ropensci.org/vcr/>

- Report bugs at <https://github.com/ropensci/vcr/issues>

## Author

**Maintainer**: Scott Chamberlain <myrmecocystus@gmail.com>
([ORCID](https://orcid.org/0000-0003-1444-9135))

Authors:

- Scott Chamberlain <myrmecocystus@gmail.com>
  ([ORCID](https://orcid.org/0000-0003-1444-9135))

- Aaron Wolen ([ORCID](https://orcid.org/0000-0003-2542-2202))

- Maëlle Salmon ([ORCID](https://orcid.org/0000-0002-2815-0399))

- Daniel Possenriede ([ORCID](https://orcid.org/0000-0002-6738-9845))

- Hadley Wickham <hadley@posit.co>

Other contributors:

- rOpenSci ([ROR](https://ror.org/019jywm96)) \[funder\]
