# Collection Overview

In order to collect forensic data `artemis` needs a [TOML](https://toml.io/en/) collection that defines
what data should be collected. This TOML collection can either be a file or a
base64 encoded TOML file.

The core parts of a TOML collection are:

- The target OS (Windows or macOS)
- The output configuration such as output and format type
- A list of artifacts to collect
