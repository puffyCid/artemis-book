# Scripts

The easiest way to start scripting is to create a Deno project.

- `deno init <project name>` will create a project in the current directory

The [deno](https://deno.land/manual@v1.32.4/introduction) website contains the
full documentation on a Deno project layout. By default the following files are
created for a new project:

- `deno.jsonc`
- `main.ts`
- `main_bench.ts`
- `main_test.ts`

Since we are using a runtime built specifically for forensics and IR none of the
builtin Deno functions are available. All scripts must import the
[artemis-api](https://github.com/puffyCid/artemis-api) modules in order to
effectively create scripts. In addition, to `artemis-api` only the vanilla
[JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
API is available for scripting

To import `artemis` functions into your script, open `main.ts` and import the
function associated with the artifact you want to parse. For example, to parse
the Windows `Registry` you would import:

```typescript
import { getRegistry } from "https://raw.githubusercontent.com/puffycid/artemis-api/master/mod.ts";
```

If you wanted to parse the Windows `Registry` and manipulate the parsed data you
would import:

```typescript
import { getRegistry } from "https://raw.githubusercontent.com/puffycid/artemis-api/master/mod.ts";
import { Registry } from "https://raw.githubusercontent.com/puffycid/artemis-api/master/src/windows/registry.ts";
```

A list of all exported `artemis` functions can be found at
`https://github.com/puffyCid/artemis-api`. All artifacts supported by `artemis`
are callable from `TypeScript`. The structured output produced by each
`artifact` is listed in the respective `artifact` chapter. For example, the
structured `Registry` data format return `getRegistry` is found in the
[Registry chapter](../artifacts/windows/registry.md)

Once we have created and bundled our script. We just need to base64 encode
before providing it to `artemis`.

# TOML Collection

An example TOML collection would like this

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

# Collection Options

- `name` Name for script
- `script` Base64 encoded bundled script (JavaScript)
