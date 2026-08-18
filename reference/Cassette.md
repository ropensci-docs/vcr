# Cassette handler

Main R6 class that is called from the main user facing function
[`use_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)

## Value

An R6 `Cassette` pbject.

## See also

[`vcr_configure()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md),
[`use_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md),
[`insert_cassette()`](https://docs.ropensci.org/vcr/reference/insert_cassette.md)

## Public fields

- `name`:

  (character) cassette name

- `record`:

  (character) record mode

- `serialize_with`:

  (character) serializer (yaml\|json\|qs2)

- `serializer`:

  (Serializer) serializer (YAML\|JSON\|QS2)

- `match_requests_on`:

  (character) matchers to use

- `re_record_interval`:

  (numeric) the re-record interval

- `root_dir`:

  root dir, gathered from
  [`vcr_configuration()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md)

- `preserve_exact_body_bytes`:

  (logical) Whether to base64 encode the bytes of the requests and
  responses

- `http_interactions`:

  (list) internal use

- `new_interactions`:

  (boolean) Have any interactions been recorded?

- `warn_on_empty`:

  (logical) warn if no interactions recorded

- `new_cassette`:

  is this a new cassette?

## Methods

### Public methods

- [`Cassette$new()`](#method-Cassette-initialize)

- [`Cassette$insert()`](#method-Cassette-insert)

- [`Cassette$eject()`](#method-Cassette-eject)

- [`Cassette$print()`](#method-Cassette-print)

- [`Cassette$file()`](#method-Cassette-file)

- [`Cassette$recording()`](#method-Cassette-recording)

- [`Cassette$replaying()`](#method-Cassette-replaying)

- [`Cassette$remove_outdated_interactions()`](#method-Cassette-remove_outdated_interactions)

- [`Cassette$record_http_interaction()`](#method-Cassette-record_http_interaction)

- [`Cassette$clone()`](#method-Cassette-clone)

------------------------------------------------------------------------

### `Cassette$new()`

Create a new `Cassette` object

#### Usage

    Cassette$new(
      name,
      dir = NULL,
      record = NULL,
      match_requests_on = NULL,
      serialize_with = NULL,
      preserve_exact_body_bytes = NULL,
      re_record_interval = NULL,
      warn_on_empty = NULL
    )

#### Arguments

- `name`:

  The name of the cassette. vcr will sanitize this to ensure it is a
  valid file name.

- `dir`:

  The directory where the cassette will be stored.

- `record`:

  The record mode. Default: "once".

- `match_requests_on`:

  HTTP request components to use when matching.

- `serialize_with`:

  (character) Which serializer to use. Valid values are "yaml"
  (default), "json", and "qs2".

- `preserve_exact_body_bytes`:

  (logical) Whether or not to base64 encode the bytes of the requests
  and responses for this cassette when serializing it. See also
  `preserve_exact_body_bytes` in
  [`vcr_configure()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md).
  Default: `FALSE`

- `re_record_interval`:

  (numeric) When given, the cassette will be re-recorded at the given
  interval, in seconds.

- `warn_on_empty`:

  Warn when ejecting the cassette if no interactions have been recorded.

#### Returns

A new `Cassette` object

------------------------------------------------------------------------

### `Cassette$insert()`

insert the cassette

#### Usage

    Cassette$insert()

#### Returns

self

------------------------------------------------------------------------

### `Cassette$eject()`

ejects the cassette

#### Usage

    Cassette$eject()

#### Returns

self

------------------------------------------------------------------------

### `Cassette$print()`

print method for `Cassette` objects

#### Usage

    Cassette$print(x, ...)

#### Arguments

- `x`:

  self

- `...`:

  ignored

------------------------------------------------------------------------

### `Cassette$file()`

get the file path for the cassette

#### Usage

    Cassette$file()

#### Returns

character

------------------------------------------------------------------------

### `Cassette$recording()`

Is the cassette in recording mode?

#### Usage

    Cassette$recording()

#### Returns

logical

------------------------------------------------------------------------

### `Cassette$replaying()`

Is the cassette in replaying mode?

#### Usage

    Cassette$replaying()

#### Returns

logical

------------------------------------------------------------------------

### `Cassette$remove_outdated_interactions()`

Remove outdated interactions

#### Usage

    Cassette$remove_outdated_interactions()

------------------------------------------------------------------------

### `Cassette$record_http_interaction()`

record an http interaction (doesn't write to disk)

#### Usage

    Cassette$record_http_interaction(request, response)

#### Arguments

- `request`:

  A `vcr_request`.

- `response`:

  A `vcr_response`.

#### Returns

an interaction as a list with request and response slots

------------------------------------------------------------------------

### `Cassette$clone()`

The objects of this class are cloneable with this method.

#### Usage

    Cassette$clone(deep = FALSE)

#### Arguments

- `deep`:

  Whether to make a deep clone.
