# Spec

## Summary

This spec describes the three real, assertion-based tests that
`src/demo.rs` implements against Google Maps's syntax and interaction
pattern, using the `thirtyfour` crate. It matches the sibling
`demo-selenium-javascript-for-google-maps` and
`demo-selenium-python-for-google-maps` repos test-for-test.

## Scope

This spec covers `src/demo.rs`: the target URL, every selector it uses,
and every assertion it makes. It does NOT cover installation (see
README.md) or CI/build tooling — there is none, deliberately, per
AGENTS.md.

## Principles and rules

- Google's Terms of Service restrict automated querying of its services,
  including Google Maps. This code is a syntax/pattern reference. It must
  never be compiled, checked, or run against the live Google Maps — see
  AGENTS.md.
- The code and this spec describe the same scenario. If they ever diverge,
  that is a defect — fix it before making any other change.
- This repo depends on the crate published on crates.io as `thirtyfour`,
  the de facto Selenium/WebDriver client for Rust (there is no official
  Selenium Rust binding).
- Most of the Google Maps canvas is rendered to `<canvas>`/WebGL, not
  addressable DOM. These tests only touch the chrome around the map (the
  search box, the zoom button) and the URL, which Google Maps keeps in
  sync with the current view.

## Detail

Target URL: `https://www.google.com/maps`

1. **Title test** (`google_maps_title_contains_google_maps`)
   * Navigate to `https://www.google.com/maps`.
   * Assert `driver.title()` contains the substring `Google Maps`.

2. **Search test** (`google_maps_search_url_contains_query`)
   * Navigate to `https://www.google.com/maps`.
   * Locate the search box via
     `driver.query(By::Css("[aria-label='Search Google Maps']")).single()`
     and send it the query `Cardiff Castle` followed by the WebDriver
     "Enter" key (`\u{E007}`) via `.send_keys(...)`.
   * Assert the resulting URL (`driver.current_url()`) contains each word
     of the query, matched case-insensitively (`cardiff`, `castle`).

3. **Zoom test** (`google_maps_zoom_in_changes_url_zoom_parameter`)
   * Navigate to `https://www.google.com/maps`, search for
     `Cardiff Castle` as in test 2.
   * Read the zoom level embedded in the URL's `@lat,lng,zoomz` segment.
   * Click the zoom-in control via
     `driver.query(By::Css("[aria-label='Zoom in']")).single()`.
   * Read the zoom level again and assert it increased.

## Acceptance criteria

- All three test functions above compile against `thirtyfour` 0.36's
  documented API shape (verified by manual review, and by a throwaway
  scratch compile of the same API calls outside this repo — see Sources;
  not by running `cargo build`/`cargo check`/`cargo test` in this repo).
- None of the three tests is ever executed against the live
  `https://www.google.com/maps`.

## Related topics

- [../README.md](../README.md)
- [../AGENTS.md](../AGENTS.md)

## Sources

- [https://www.google.com/policies/terms/](https://www.google.com/policies/terms/)
- [https://testingexamples.github.io/examples/google-maps/](https://testingexamples.github.io/examples/google-maps/)
- [https://crates.io/crates/thirtyfour](https://crates.io/crates/thirtyfour)
