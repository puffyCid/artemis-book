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

Since we are using an embedded version of Deno in `artemis` we usually
cannot<sup>*</sup> benchmark or test our scripts, so you can delete/ignore
`main_bench.ts` and `main_test.ts`

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
`https://raw.githubusercontent.com/puffycid/artemis-api/master/mod.ts`. All
artifacts supported by `artemis` are callable from `TypeScript`. The structured
output produced by each `artifact` is listed in the respective `artifact`
chapter. For example, the structured `Registry` data format return `getRegistry`
is found in the [Registry chapter](../artifacts/windows/registry.md)

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

## <sup>*</sup> Vanilla Scripts

The exception to this rule is if we do not call any `artemis` functions. If you
make a script that uses the regular ("vanilla") Deno API and/or third-party Deno
modules, then you can test and benchmark your script since it only calls APIs
supported by the official Deno platform (vs the extra functions registered by
`artemis`)

The example script below parses all installed macOS Homebrew packages using the
vanilla Deno API and therefore can be run via `deno run main.ts` in addition to
`artemis`.

```typescript
interface HomebrewReceipt extends HomebrewFormula {
  installedAsDependency: boolean;
  installedOnRequest: boolean;
  installTime: number;
  sourceModified: number;
  version: string;
  name: string;
}

interface HomebrewFormula {
  description: string;
  homepage: string;
  url: string;
  license: string;
  caskName: string;
  formulaPath: string;
}

interface HomebrewData {
  packages: HomebrewReceipt[];
  casks: HomebrewFormula[];
}

/**
 * Class used to parse `.rb` and `json` files related to installed `Homebrew` packges
 */
class Homebrew {
  private desc = /(?<=desc ).*$/m;
  private homepage_reg = /(?<=homepage ).*$/m;
  private reg_license = /(?<=license ).*$/m;
  private reg_url = /(?<=url ).*$/m;
  private reg_name = /(?<=name ).*$/m;

  /**
   * @param path Path to formula .rb file to read
   * @param receipt HomebrewReceipt object to modify
   * @returns Updated HomebrewReceipt object
   */
  public readRuby(path: string): HomebrewFormula {
    const rubyText = Deno.readTextFileSync(path);
    const descriptoin = rubyText.match(this.desc);
    const receipt: HomebrewFormula = {
      description: "",
      homepage: "",
      url: "",
      license: "",
      caskName: "",
      formulaPath: path,
    };
    if (typeof descriptoin?.[0] === "string") {
      receipt.description = descriptoin?.[0].replaceAll('"', "");
    }

    const homepage = rubyText.match(this.homepage_reg);
    if (typeof homepage?.[0] === "string") {
      receipt.homepage = homepage?.[0].replaceAll('"', "");
    }

    const license = rubyText.match(this.reg_license);
    if (typeof license?.[0] === "string") {
      receipt.license = license?.[0].replaceAll('"', "");
    }

    const url = rubyText.match(this.reg_url);
    if (typeof url?.[0] === "string") {
      receipt.url = url?.[0].replaceAll('"', "");
    }

    const name = rubyText.match(this.reg_name);
    if (typeof name?.[0] === "string") {
      receipt.caskName = name?.[0].replaceAll('"', "");
    }

    return receipt;
  }

  /**
   * @param path Path to formula INSTALL_RECEIPT.json file to read
   * @param name Homebrew formula name
   * @returns HomebrewReceipt object
   */
  public readReceipts(
    path: string,
    name: string,
    formula: HomebrewFormula,
  ): HomebrewReceipt {
    const data = Deno.readTextFileSync(path);
    const jsonData = JSON.parse(data);
    const receipt: HomebrewReceipt = {
      installedAsDependency: jsonData["installed_as_dependency"],
      installedOnRequest: jsonData["installed_on_request"],
      installTime: jsonData["time"],
      sourceModified: jsonData["source_modified_time"],
      version: jsonData["source"]["versions"]["stable"],
      formulaPath: jsonData["source"]["path"],
      name: name,
      description: formula.description,
      homepage: formula.homepage,
      url: formula.url,
      license: formula.license,
      caskName: "N/A",
    };
    return receipt;
  }
}

/**
 * Get information on all installed `Homebrew Casks`
 */
function listCasks(): Array<HomebrewFormula> {
  const path = "/usr/local/Caskroom";
  const casks: Array<HomebrewFormula> = [];

  for (const entry of Deno.readDirSync(path)) {
    if (!entry.isDirectory) {
      continue;
    }

    const caskName = `${path}/${entry.name}/.metadata`;
    const caskData = new Homebrew();

    for (const versionEntry of Deno.readDirSync(caskName)) {
      if (!versionEntry.isDirectory) {
        continue;
      }
      const caskName = `${path}/${entry.name}/.metadata/${versionEntry.name}`;

      for (const timeVersion of Deno.readDirSync(caskName)) {
        if (!versionEntry.isDirectory) {
          continue;
        }
        const formulaPath =
          `${path}/${entry.name}/.metadata/${versionEntry.name}/${timeVersion.name}/Casks/${entry.name}.rb`;
        const formulaInfo = Deno.lstatSync(formulaPath);
        if (formulaInfo.isFile) {
          let receipt: HomebrewFormula = {
            description: "",
            homepage: "",
            url: "",
            license: "",
            caskName: "",
            formulaPath,
          };
          receipt = caskData.readRuby(formulaPath);
          casks.push(receipt);
        }
      }
    }
  }
  return casks;
}

/**
 * Get information on all installed `Homebrew Formulas`
 */
function listReceipts(): Array<HomebrewReceipt> {
  const path = "/usr/local/Cellar";
  const brew_receipts: Array<HomebrewReceipt> = [];

  for (const entry of Deno.readDirSync(path)) {
    if (!entry.isDirectory) {
      continue;
    }
    const brew_name = `${path}/${entry.name}`;
    const brewData = new Homebrew();
    for (const brew_entry of Deno.readDirSync(brew_name)) {
      if (!brew_entry.isDirectory) {
        continue;
      }

      const receiptPath =
        `${path}/${entry.name}/${brew_entry.name}/INSTALL_RECEIPT.json`;
      const formulaPath =
        `${path}/${entry.name}/${brew_entry.name}/.brew/${entry.name}.rb`;

      const receipt_info = Deno.lstatSync(receiptPath);
      if (!receipt_info.isFile) {
        continue;
      }
      const formulaInfo = Deno.lstatSync(formulaPath);
      if (!formulaInfo.isFile) {
        continue;
      }
      const formula = brewData.readRuby(formulaPath);
      const receipt = brewData.readReceipts(
        receiptPath,
        entry.name,
        formula,
      );

      brew_receipts.push(receipt);
    }
  }
  return brew_receipts;
}

/**
 * Entry function for `Deno`!
 * Parses Homebrew info without using any `Artemis` functions
 */
function main(): HomebrewData {
  const brew = listReceipts();
  const casks = listCasks();
  const homebrew: HomebrewData = {
    packages: brew,
    casks: casks,
  };
  return homebrew;
}

main();
```
