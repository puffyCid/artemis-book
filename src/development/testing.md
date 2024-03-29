# Testing

`artemis` has a single basic guideline for testing:

- All functions should ideally have a test

For example, if you open a pull request to add a new feature and create three
(3) new functions. Your must have a test for each new function (three tests
total). Even if your functions are coded like:
`functionA calls functionB which then call functionC`. You must have tests for
each function and not just functionA.

To run tests

```sh
# Its recommended to run in release mode for tests. This will speed up the tests
cargo test --release
```

If you are unfamilar with creating Rust tests. The Rust
[book](https://doc.rust-lang.org/book/ch11-03-test-organization.html) and
[Rust by example](https://doc.rust-lang.org/rust-by-example/testing/unit_testing.html)
have great learning resources.

## Integration Tests

If you are adding a new forensic aritfact to `artemis`, including an integration
test for the artifact can also be very useful. Writing an integration is a two
(2) step process:

1. Create a TOML collection file. This should be TOML collection file that
   anyone could download and run themselves
2. Create a `artifact_tester.rs` file

An example `prefetch` integration test:

1. TOML file created at
   `<path to repo>/artemis-core/tests/test_data/windows/prefetch.toml`

```toml
system = "windows"

[output]
name = "prefetch_collection"
directory = "./tmp"
format = "json"
compress = false
endpoint_id = "6c51b123-1522-4572-9f2a-0bd5abd81b82"
collection_id = 1
output = "local"

[[artifacts]]
artifact_name = "prefetch"
[artifacts.prefetch]
alt_drive = 'C'
```

2. `prefetch_tester.rs` created at
   `<path to repo>/artemis-core/tests/prefetch_tester.rs`

```rust
#[test]
#[cfg(target_os = "windows")]
fn test_prefetch_parser() {
    use std::path::PathBuf;

    use artemis_core::core::parse_toml_file;
    let mut test_location = PathBuf::from(env!("CARGO_MANIFEST_DIR"));
    test_location.push("tests/test_data/windows/prefetch.toml");

    let results = parse_toml_file(&test_location.display().to_string()).unwrap();
    assert_eq!(results, ())
}
```

Our `prefetch_tester.rs` file runs the `prefetch.toml` file through the whole
`artemis` program.

## Test Data

If you are adding a new forensic artifact to `artemis`, if you can include a
sample of the artifact that can be used for tests that would be very helpful.
Some things to keep in mind though:

- Size. If the artifact is large (10-20MB) then including the sample in the
  `artemis` repo is unecessary.
- Licensing. If you can provide the artifact from your own system that is ideal.
  However, if you find the sample aritfact in another GitHub repo make sure that
  repo's LICENSE is compatible with `artemis`.

### Hypothetical Scenario

Lets say in a future version of Windows (ex: Windws 14) Microsoft decides to
update the [prefech](../artifacts/windows/prefetch.md) file format. You want to
add support for this updated format in `artemis`. The process would be something
like:

1. Create an issue describing what you want to do. Ex:
   `Issue: Add support for Windows 14 Prefetch file`
2. Clone the `artemis` source code
3. Create a new branch
4. Code and add the feature. Since this is related to Windows `prefetch` you
   would probably be working in:
   \
   `artemis-core/src/artifacts/os/windows/prefetch/versions/<prefetch version>.rs`
5. Add tests for any new functions you add
6. Add one (1) or more Windows 14 sample `prefetch` files to:
   \
   `artemis-core/tests/test_data/windows/prefetch/win14/`
7. Run tests and verify results
8. Open pull request
