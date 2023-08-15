# Shortcuts

Windows `Shotcut` files (`.lnk`) are files that point to another file. They
often contain a large amount of metadata related to the target file. `Shortcut`
files can be used to distribute malware and can also provide evidence of file
interaction. The directory at
`C:\Users\%\AppData\Roaming\Microsoft\Windows\Recent` contains multiple
`Shortcuts` that point to files recently opened by the user.

Due to format complexity and verbose details `artemis` does not currently parse
the whole format, only the data most useful for forensic analysis. If additional
data should be included please open a GitHub issue.

Other Parsers:

- [LECmd](https://ericzimmerman.github.io/)
- [Velociraptor](https://docs.velociraptor.app/artifact_references/pages/windows.forensics.lnk/)

References:

- [Libyal](https://github.com/libyal/liblnk/blob/main/documentation/Windows%20Shortcut%20File%20(LNK)%20format.asciidoc)

# TOML Collection

```toml
system = "windows"

[output]
name = "shortcuts_collection"
directory = "./tmp"
format = "json"
compress = false
endpoint_id = "6c51b123-1522-4572-9f2a-0bd5abd81b82"
collection_id = 1
output = "local"

[[artifacts]]
artifact_name = "shortcuts"
[artifacts.shortcuts]
path = "C:\\ProgramData\\Microsoft\\Windows\\Start Menu\\Programs\\Startup"
```

# Collection Options

- `path` Target path where `artemis` should parse `Shortcut` files. This
  configuration is **required**

# Output Structure

A `Shortcut` object structure

```typescript
export interface Shortcut {
  /**Path to `shortcut (lnk)` file */
  source_path: string;
  /**Flags that specify what data structures are in the `lnk` file */
  data_flags: string[];
  /**File attributes of target file */
  attribute_flags: string[];
  /**Standard Information created timestamp of target file */
  created: number;
  /**Standard Information accessed timestamp of target file */
  accessed: number;
  /**Standard Information modified timestamp of target file */
  modified: number;
  /**Size in bytes of target file */
  file_size: number;
  /**Flag associated where target file is located. On volume or network share */
  location_flags: string;
  /**Path to target file */
  path: string;
  /**Serial associated with volume if target file is on drive */
  drive_serial: string;
  /**Drive type associated with volume if target file is on drive */
  drive_type: string;
  /**Name of volume if target file is on drive */
  volume_label: string;
  /**Network type if target file is on network share */
  network_provider: string;
  /**Network share name if target file is on network share */
  network_share_name: string;
  /**Network share device name if target file is on network share */
  network_device_name: string;
  /**Description of shortcut (lnk) file */
  description: string;
  /**Relative path to target file */
  relative_path: string;
  /**Directory of target file */
  working_directory: string;
  /**Command args associated with target file */
  command_line_args: string;
  /**Icon path associated with shortcut (lnk) file */
  icon_location: string;
  /**Hostname of target file */
  hostname: string;
  /**
   * Digital Record Object Identification (DROID) used to track lnk file
   */
  droid_volume_id: string;
  /**
   * Digital Record Object Identification (DROID) used to track lnk file
   */
  droid_file_id: string;
  /**
   * Digital Record Object Identification (DROID) used to track lnk file
   */
  birth_droid_volume_id: string;
  /**
   * Digital Record Object Identification (DROID) used to track lnk file
   */
  birth_droid_file_id: string;
  /**Shellitems associated with shortcut (lnk) file */
  shellitems: ShellItems[];
  /**Root guid for a property store */
  property_guid: string;
}
```
