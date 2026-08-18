# Package index

## Main functions

Probably the only functions you’ll need

- [`use_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
  [`local_cassette()`](https://docs.ropensci.org/vcr/reference/use_cassette.md)
  : Use a cassette to record HTTP requests
- [`vcr_configure()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md)
  [`local_vcr_configure()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md)
  [`vcr_configure_reset()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md)
  [`vcr_configuration()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md)
  [`vcr_config_defaults()`](https://docs.ropensci.org/vcr/reference/vcr_configure.md)
  : Global Configuration Options
- [`vcr_configure_log()`](https://docs.ropensci.org/vcr/reference/vcr_configure_log.md)
  [`local_vcr_configure_log()`](https://docs.ropensci.org/vcr/reference/vcr_configure_log.md)
  : Configure vcr logging
- [`insert_example_cassette()`](https://docs.ropensci.org/vcr/reference/insert_example_cassette.md)
  : Use cassettes in examples
- [`setup_knitr()`](https://docs.ropensci.org/vcr/reference/setup_knitr.md)
  : Use vcr in vignettes

## Managing cassettes

- [`cassettes()`](https://docs.ropensci.org/vcr/reference/cassettes.md)
  [`current_cassette()`](https://docs.ropensci.org/vcr/reference/cassettes.md)
  [`current_cassette_recording()`](https://docs.ropensci.org/vcr/reference/cassettes.md)
  [`current_cassette_replaying()`](https://docs.ropensci.org/vcr/reference/cassettes.md)
  [`cassette_path()`](https://docs.ropensci.org/vcr/reference/cassettes.md)
  : List cassettes, get current cassette, etc.
- [`delete_cassettes()`](https://docs.ropensci.org/vcr/reference/delete_cassettes.md)
  : Delete cassettes by prefix
- [`is_recording()`](https://docs.ropensci.org/vcr/reference/is_recording.md)
  [`is_replaying()`](https://docs.ropensci.org/vcr/reference/is_recording.md)
  : Determine if vcr is recording/replaying

## Other helpers

- [`turn_on()`](https://docs.ropensci.org/vcr/reference/lightswitch.md)
  [`turn_off()`](https://docs.ropensci.org/vcr/reference/lightswitch.md)
  [`turned_off()`](https://docs.ropensci.org/vcr/reference/lightswitch.md)
  [`turned_on()`](https://docs.ropensci.org/vcr/reference/lightswitch.md)
  [`skip_if_vcr_off()`](https://docs.ropensci.org/vcr/reference/lightswitch.md)
  : Turn vcr on and off
- [`vcr_test_path()`](https://docs.ropensci.org/vcr/reference/vcr_test_path.md)
  : Locate file in tests directory
- [`vcr_last_request()`](https://docs.ropensci.org/vcr/reference/vcr_last_request.md)
  [`vcr_last_response()`](https://docs.ropensci.org/vcr/reference/vcr_last_request.md)
  : Retrieve last vcr request/response
