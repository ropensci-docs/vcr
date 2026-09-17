# Use vcr in vignettes

`setup_knitr()` registers a knitr hook to make it easy to use vcr inside
a vignette. First call `setup_knitr()` in your setup chunk (or other
chunk early in the document):

    ```{r setup}
    #| include: false
    vcr::setup_knitr()
    ```

Then, in a chunk where you want to use a cassette, set the `cassette`
chunk option to the name of the cassette:

    ```{r}
    #| cassette: name
    req <- httr2::request("http://r-project.org")
    resp <- httr2::req_perform(req)
    ```

Or if you have labelled the chunk, you can use `cassette: true` to use
the chunk label as the cassette name:

    ```{r}
    #| label: cassette-name
    #| cassette: true
    req <- httr2::request("http://r-project.org")
    resp <- httr2::req_perform(req)
    ```

You can build your vignettes however you usually do (from the command
line, with pkgdown, with RStudio/Positron, etc).

## Usage

``` r
setup_knitr(prefix = NULL, dir = "_vcr", ...)
```

## Arguments

- prefix:

  An prefix for the cassette name to make cassettes unique across
  vignettes. Defaults to the file name (without extension) of the
  currently executing vignette.

- dir:

  Directory where to create the cassette file. Default: \`"\_vcr"“.

- ...:

  Other arguments passed on to
  [`insert_cassette()`](https://docs.ropensci.org/vcr/reference/insert_cassette.md).
