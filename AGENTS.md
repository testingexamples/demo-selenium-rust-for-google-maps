# AGENTS.md

This repo is a small Rust test suite, written for the `thirtyfour` crate,
that demonstrates real Selenium assertions against Google Maps's syntax
and interaction pattern — matching the sibling
`demo-selenium-javascript-for-google-maps` and
`demo-selenium-python-for-google-maps` repos test-for-test.

`spec/index.md` is the single source of truth for the exact three test
scenarios, selectors, and assertions this demo describes. If the code in
`src/demo.rs` and `spec/index.md` ever disagree, that is a defect in one
of them — fix it before doing anything else.

## Non-negotiable: never execute against live google.com

Google's Terms of Service restrict automated querying of its services,
including Google Maps. Do **not**:

* Run `cargo build`, `cargo check`, or `cargo test` in this repo.
* Add a CI workflow, pre-commit hook, or any other automation that
  compiles or executes this code.
* Make any live network request to `google.com` from this repo, in any
  form.

This code exists purely to demonstrate `thirtyfour` syntax and interaction
patterns, matching the sibling JavaScript/Python/TypeScript
`-for-google-maps` demos. Review changes by careful manual reading against
the crate's confirmed API shape (see README.md and `spec/index.md`'s
Sources), not by compiling or running them.

## Crate choice

`thirtyfour` is the crate this repo's `Cargo.toml` names — the de facto
Selenium/WebDriver client for Rust (there is no official Selenium Rust
binding).

CLAUDE.md is a pointer to this file — it is the single source of truth for
agent instructions.
