# Development Overview

The `artemis` source code is about ~66k lines of Rust code across ~540 files as
of September 2023 (this includes tests), however its organized in a pretty
simple manner.

From the root of the `artemis` repo:

- `/artemis-core` workspace containsthe library component of `artemis`. The bulk
  of the code is located here
- `/cli` workspace contains the executable component `artemis`. This is very
  small.
- `/server` workspace contains the experimental server component of `artemis`.
  Its currently very bare bones

From `/artemis-core` directory:

- `/src` contains the source code of `artemis-core`.
- `/tests` contains test data and integration tests
- `/tmp` output directory for all tests (if you choose to run them)

From the `artemis-core/src/` directory

- `/artifacts` contains the code related to parsing forensic artifacts. It is
  broken down by OS and application artifacts
- `/filesystem` contains code to help interact with the filesystem. It contains
  helper functions that can be used when adding new artifacts/features. Ex:
  reading/hashing files, getting file timestamps, listing files, etc
- `/output` contains code related to outputing parsed data
- `/runtime` contains code related to the embedded Deno runtime
- `/structs` contains code related to how TOML collection files are parsed. It
  tells `artemis` how to interpret TOML collections.
- `/utils` contains code related to help parse artifacts and provide other
  features to `artemis`. Ex: Decompress/compress data, get environment
  variables, create a Regex expression, extract strings, convert timestamps, etc
- `core.rs` contains the entry point to the `artemis_core` library.

# Adding New Artifacts

To keep the codebase organized the follow should be followed when adding a new
artifact.

- Artifacts have their own subfolder. Ex: `src/artifacts/os/windows/prefetch`
- The subfolder should have the following files at least:
  - `parser.rs` - Contains the `pub(crate)` accessible functions for the
    artifact
  - `error.rs` - Artifact specific errors

# Timestamps

All timestamps `artemis` outputs are in UNIXEPOCH seconds. The only exceptions
are:

- `UnifiedLogs` and `EventLogs` use UNIXEPOCH nanoseconds.
- `Journals` use UNIXEPOCH microseconds.

If your new artifact has a timestamp, you will need to make sure the timestamp
is in UNIXEPOCH seconds. Though exceptions may be allowed if needed, these
exceptions will only be for the duration (ex: seconds vs nanoseconds).\
No other time formats such as Windows FILETIME, FATTIME, Chromium time, etc are
allowed.

# Artifact Scope

Currently all artifacts that `artemis` parses are statically coded in the binary
(they are written in Rust). While this ok, it prevents us from dyanamically
upating the parser if the artifact format changes (ex: new Windows release).

Currently the [JS runtime](../scripting/deno.md) has minimal support for
creating parsers. If you are interested in adding a small parser to `artemis`,
it could be worth first trying to code it using the JS runtime.\
An example JS runtime parser can be found in the
[artemis API](https://github.com/puffyCid/artemis-api/blob/main/src/macos/alias.ts)
repo.

However, if you want to implement a new parser for parsing common Windows
artifacts such as `Jumplists` then that is definitely something that could be
worth including as a static parser.

When in doubt or unsure open an issue!

# Suggestions

If you want add a new artifact but want to see how other artifacts are
implemented, some suggested ones to review are:

- `UserAssist`: If you want to add a new Registry artifact. The `UserAssist`
  artifact is less than 300 lines (not counting tests). And includes:
  - Parsing binary data
  - Converting timestamps
  - Collecting user Registy data
- `FsEvents`: If you want to to parse a binary file. The `FsEvents` is less than
  300 lines (not counting tests). And includes:
  - Parsing binary data
  - Decompressing data
  - Getting data flags
    \
    Fun fact: `FsEvents` was the first artifact created for `artemis`. Its the
    oldest code in the project!

## Suggested Helper Functions

The `artemis` codebase contains a handful of artifacts (ex: `Registry`) that
expose helper functions that allow other artifacts to reuse parts of that
artifact to help get artifact specific data.

For example the Windows `Registry` artifact exposes a helper function that other
`Registry` based artifacts can leverage to help parse the `Registry`:

- `pub(crate) fn get_registry_keys(start_path: &str, regex: &Regex, file_path: &str)`
  will read a `Registry` at provide `file_path` and filter to based on
  `start_path` and `regex`. If `start_path` and `regex` are empty a full
  `Registry` listing is returned. All Regex comparisons are done in lowercase.

Some other examples listed below:

- `/filesytem` contains code to help interact with the filesystem.
  - `pub(crate) fn list_files(path: &str)` returns list of files
  - `pub(crate) fn read_file(path: &str)` reads a file
  - `pub(crate) fn hash_file(hashes: &Hashes, path: &str)` hashes a file based
    on select hashes (MD5, SHA1, SHA256)

- `/filesystem/ntfs` contains code to help iteract with the raw NTFS filesystem.
  It lets us bypass locked files. This is only available on Windows
  - `pub(crate) fn raw_read_file(path: &str)` reads a file. Will bypass file
    locks
  - `pub(crate) fn read_attribute(path: &str, attribute: &str)` can read an
    Alternative Data Stream (ADS)
  - `pub(crate) fn get_user_registry_files(drive: &char)` returns a Vector that
    contains references to all user Registry files (NTUSER.DAT and
    UsrClass.dat). It does **not** read the files, it just provides all the data
    needed to start reading them.

- `/src/artifacts/os/macos/plist/property_list.rs` contains code help parse
  `plist` files.
  - `pub(crate) fn parse_plist_file(path: &str)` will parse a `plist` file and
    return it as a Serde Value
  - `pub(crate) fn parse_plist_file_dict(path: &str)` will parse a `plist` file
    and return as Dictionary for further parsing by the caller
