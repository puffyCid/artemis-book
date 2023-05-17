## Filter Scripts

In addition to creating scripts that call `artemis` functions. `artemis` has the
ability to pass the artifact data as an argument to a script! For most scenarios
calling the `artemis` function is the recommended practice for scripting.
However, the sole execption is the `filelisting` and `rawfilelisting` artifacts.

When pulling a filelisting `artemis` will recursively walk the filesystem, but
in order to keep memory usage low, every 100,000 files `artemis` will output the
results. While this will keep memory usage low, it makes it difficult to use via
scripting. If we return 100,000 entries to our script, we cannot continue our
recursive filelisting because we have lost track where we are in the filesystem.

This where filter scripts can help.

Instead of calling an `artemis` function like `getRegistry` we instead tell
`artemis` to pass the artifact data as an argument to our script. So, instead of
returning 100,000 files, we pass that data as an argument to our script before
outputting the results.

A normal `artemis` script would look like something below:

```toml
system = "macos"

[output]
name = "plist_data"
directory = "./tmp"
format = "json"
compress = false
endpoint_id = "6c51b123-1522-4572-9f2a-0bd5abd81b82"
collection_id = 1
output = "local"

[[artifacts]]
artifact_name = "script"
[artifacts.script]
name = "all_users_plist_files"
# Parses all plist files in /Users/%
script = "ZnVuY3Rpb24gZ2V0X3BsaXN0KHBhdGgpIHsKICAgIGNvbnN0IGRhdGEgPSBEZW5vLmNvcmUub3BzLmdldF9wbGlzdChwYXRoKTsKICAgIGlmIChkYXRhID09PSAiIikgewogICAgICAgIHJldHVybiB7CiAgICAgICAgICAgICJwbGlzdF9lcnJvciI6IGBmYWlsZWQgdG8gcGFyc2UgcGxpc3QgYXQgcGF0aDogJHtwYXRofWAKICAgICAgICB9OwogICAgfQogICAgY29uc3QgbG9nX2RhdGEgPSBKU09OLnBhcnNlKGRhdGEpOwogICAgcmV0dXJuIGxvZ19kYXRhOwp9CmZ1bmN0aW9uIGdldFBsaXN0KHBhdGgpIHsKICAgIHJldHVybiBnZXRfcGxpc3QocGF0aCk7Cn0KZnVuY3Rpb24gbWFpbigpIHsKICAgIGNvbnN0IHN0YXJ0X3BhdGggPSAiL1VzZXJzIjsKICAgIGNvbnN0IHBsaXN0X2ZpbGVzID0gW107CiAgICByZWN1cnNlX2RpcihwbGlzdF9maWxlcywgc3RhcnRfcGF0aCk7CiAgICByZXR1cm4gcGxpc3RfZmlsZXM7Cn0KZnVuY3Rpb24gcmVjdXJzZV9kaXIocGxpc3RfZmlsZXMsIHN0YXJ0X3BhdGgpIHsKICAgIGZvciAoY29uc3QgZW50cnkgb2YgRGVuby5yZWFkRGlyU3luYyhzdGFydF9wYXRoKSl7CiAgICAgICAgY29uc3QgcGxpc3RfcGF0aCA9IGAke3N0YXJ0X3BhdGh9LyR7ZW50cnkubmFtZX1gOwogICAgICAgIGlmIChlbnRyeS5pc0ZpbGUgJiYgZW50cnkubmFtZS5lbmRzV2l0aCgiLnBsaXN0IikpIHsKICAgICAgICAgICAgY29uc3QgZGF0YSA9IGdldFBsaXN0KHBsaXN0X3BhdGgpOwogICAgICAgICAgICBjb25zdCBwbGlzdF9pbmZvID0gewogICAgICAgICAgICAgICAgcGxpc3RfY29udGVudDogZGF0YSwKICAgICAgICAgICAgICAgIGZpbGU6IHBsaXN0X3BhdGgKICAgICAgICAgICAgfTsKICAgICAgICAgICAgcGxpc3RfZmlsZXMucHVzaChwbGlzdF9pbmZvKTsKICAgICAgICAgICAgY29udGludWU7CiAgICAgICAgfQogICAgICAgIGlmIChlbnRyeS5pc0RpcmVjdG9yeSkgewogICAgICAgICAgICByZWN1cnNlX2RpcihwbGlzdF9maWxlcywgcGxpc3RfcGF0aCk7CiAgICAgICAgfQogICAgfQp9Cm1haW4oKTs="
```

High level overview of what happens:\
`TOML file -> decode script -> artemis executes script -> output data`

A filter script would look like something below:

```toml
system = "macos"

[output]
name = "info_plist_collection"
directory = "./tmp"
format = "json"
compress = false
endpoint_id = "abdc"
collection_id = 1
output = "local"
filter_name = "apps_info_plists"
# This script will take the files artifact below and filter it to only return Info.plist files
# We could expand this even further by then using the plist parser on the Info.plist path and include that parsed data too
filter_script = "Ly8gZGVuby1mbXQtaWdub3JlLWZpbGUKLy8gZGVuby1saW50LWlnbm9yZS1maWxlCi8vIFRoaXMgY29kZSB3YXMgYnVuZGxlZCB1c2luZyBgZGVubyBidW5kbGVgIGFuZCBpdCdzIG5vdCByZWNvbW1lbmRlZCB0byBlZGl0IGl0IG1hbnVhbGx5CgpmdW5jdGlvbiBtYWluKCkgewogICAgY29uc3QgYXJncyA9IERlbm8uYXJnczsKICAgIGlmIChhcmdzLmxlbmd0aCA9PT0gMCkgewogICAgICAgIHJldHVybiBbXTsKICAgIH0KICAgIGNvbnN0IGRhdGEgPSBKU09OLnBhcnNlKGFyZ3NbMF0pOwogICAgY29uc3QgZmlsdGVyX2ZpbGVzID0gW107CiAgICBmb3IgKGNvbnN0IGVudHJ5IG9mIGRhdGEpewogICAgICAgIGlmIChlbnRyeS5maWxlbmFtZSA9PT0gIkluZm8ucGxpc3QiKSB7CiAgICAgICAgICAgIGZpbHRlcl9maWxlcy5wdXNoKGVudHJ5KTsKICAgICAgICB9CiAgICB9CiAgICByZXR1cm4gZmlsdGVyX2ZpbGVzOwp9Cm1haW4oKTsKCg=="

[[artifacts]]
artifact_name = "files" # Name of artifact
filter = true
[artifacts.files]
start_path = "/System/Volumes/Data/Applications" # Start of file listing
depth = 100 # How many sub directories to descend
metadata = false # Get executable metadata
md5 = false # MD5 all files
sha1 = false # SHA1 all files
sha256 = false # SHA256 all files
path_regex = "" # Regex for paths
file_regex = "" # Regex for files
```

The biggest differences are:

- We use a `[[artifacts]]` list to parse our data
- We base64 encode our script and assign to `filter_script` to tell `artemis`:
  take the results of the `[[artifacts]]` list and filter them before outputting
  the data
- We then set the `filter` value to `true`

High level overview of what happens:\
`TOML file -> walkthrough artifacts list -> artemis collects data -> pass data to filter script -> output data`\
All entries in a `[[artifacts]]` list can be sent through a filter script with
the exception of regular `artemis` scripts. The output of these scripts do not
go through `filter_script`.

The `TypeScript` code for a filter script would be something like below:

```typescript
import { MacosFileInfo } from "../../artemis-api/src/macos/files.ts";

/**
 * Filters a provided file listing argument to only return Info.plist files from /Applications
 * Two arguments are always provided:
 *   - First is the parsed data serialized into JSON string
 *   - Second is the artifact name (ex: "amcache")
 * @returns Array of files only containing Info.plist
 */
function main() {
  // Since this is a filter script our data will be passed as a Serde Value that is a string
  const args = Deno.args;
  if (args.length === 0) {
    return [];
  }

  // Parse the provide Serde Value (JSON string) as a MacosFileInfo[]
  const data: MacosFileInfo[] = JSON.parse(args[0]);
  const filter_files: MacosFileInfo[] = [];

  for (const entry of data) {
    if (entry.filename == "Info.plist") {
      filter_files.push(entry);
    }
  }
  return filter_files;
}

main();
```

The key difference between a regular `artemis` script and a filter script is:

```typescript
const args = Deno.args;
if (args.length === 0) {
  return [];
}

// Parse the provide Serde Value (JSON string) as a MacosFileInfo[]
const data: MacosFileInfo[] = JSON.parse(args[0]);
```

Here we are taking the first argument provided to our script and parsing it as a
JSON `MacosFileInfo` object array. As stated above, `artemis` will pass the
results of each `[[artifacts]]` entry to our script using serde to serialize the
data as a JSON formattted string.

We then parse and filter the data based on our script

```typescript
// Parse the provide Serde Value (JSON string) as a MacosFileInfo[]
const data: MacosFileInfo[] = JSON.parse(args[0]);
const filter_files: MacosFileInfo[] = [];

for (const entry of data) {
  if (entry.filename == "Info.plist") {
    filter_files.push(entry);
  }
}
```

Finally, we take our filtered output and return it back to `artemis`

```typescript
return filter_files;
```

So our initial data provided to our filter script gets filtered and returned. In
this example, our 100,000 file listing entry gets filtered to only return
entries with the filename `Info.plist`.
