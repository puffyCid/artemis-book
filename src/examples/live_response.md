# Live Response

Windows TOML collection focusing on collecting data to help investigate a
Windows incident.

```toml
system = "windows"

[output]
name = "windows_collection"
directory = "./tmp"
format = "jsonl"
compress = true
endpoint_id = "6c51b123-1522-4572-9f2a-0bd5abd81b82"
collection_id = 1
output = "local"

[[artifacts]]
artifact_name = "prefetch"
[artifacts.prefetch]

[[artifacts]]
artifact_name = "processes"
[artifacts.processes]
md5 = true
sha1 = false
sha256 = false
metadata = true

[[artifacts]]
artifact_name = "systeminfo"

[[artifacts]]
artifact_name = "chromium-history"

[[artifacts]]
artifact_name = "chromium-downloads"

[[artifacts]]
artifact_name = "firefox-history"

[[artifacts]]
artifact_name = "firefox-downloads"

[[artifacts]]
artifact_name = "amcache"
[artifacts.amcache]

[[artifacts]]
artifact_name = "bits"
[artifacts.bits]
carve = true

[[artifacts]]
artifact_name = "eventlogs"
[artifacts.eventlogs]

[[artifacts]]
artifact_name = "rawfiles"
[artifacts.rawfiles]
drive_letter = 'C'
start_path = "C:\\"
depth = 40
recover_indx = true
md5 = true
sha1 = false
sha256 = false
metadata = true

[[artifacts]]
artifact_name = "registry" # Parses the whole Registry file
[artifacts.registry]
user_hives = true # All NTUSER.DAT and UsrClass.dat
system_hives = true # SYSTEM, SOFTWARE, SAM, SECURITY

[[artifacts]]
artifact_name = "shellbags"
[artifacts.shellbags]
resolve_guids = true

[[artifacts]]
artifact_name = "shimcache"
[artifacts.shimcache]

[[artifacts]]
artifact_name = "srum"
[artifacts.srum]

[[artifacts]]
artifact_name = "userassist"
[artifacts.userassist]

[[artifacts]]
artifact_name = "users"
[artifacts.users]

[[artifacts]]
artifact_name = "usnjrnl"
[artifacts.usnjrnl]

[[artifacts]]
artifact_name = "script"
[artifacts.script]
name = "recent_files"
# Parses all recent accessed files (shortcuts/lnk files) for all users
script = "Ly8gZGVuby1mbXQtaWdub3JlLWZpbGUKLy8gZGVuby1saW50LWlnbm9yZS1maWxlCi8vIFRoaXMgY29kZSB3YXMgYnVuZGxlZCB1c2luZyBgZGVubyBidW5kbGVgIGFuZCBpdCdzIG5vdCByZWNvbW1lbmRlZCB0byBlZGl0IGl0IG1hbnVhbGx5CgpmdW5jdGlvbiBnZXRfbG5rX2ZpbGUocGF0aCkgewogICAgY29uc3QgZGF0YSA9IERlbm9bRGVuby5pbnRlcm5hbF0uY29yZS5vcHMuZ2V0X2xua19maWxlKHBhdGgpOwogICAgY29uc3QgbG5rID0gSlNPTi5wYXJzZShkYXRhKTsKICAgIHJldHVybiBsbms7Cn0KZnVuY3Rpb24gZ2V0TG5rRmlsZShwYXRoKSB7CiAgICByZXR1cm4gZ2V0X2xua19maWxlKHBhdGgpOwp9CmZ1bmN0aW9uIG1haW4oKSB7CiAgICBjb25zdCBkcml2ZSA9IERlbm8uZW52LmdldCgiU3lzdGVtRHJpdmUiKTsKICAgIGlmIChkcml2ZSA9PT0gdW5kZWZpbmVkKSB7CiAgICAgICAgcmV0dXJuIFtdOwogICAgfQogICAgY29uc3QgdXNlcnMgPSBgJHtkcml2ZX1cXFVzZXJzYDsKICAgIGNvbnN0IHJlY2VudF9maWxlcyA9IFtdOwogICAgZm9yIChjb25zdCBlbnRyeSBvZiBEZW5vLnJlYWREaXJTeW5jKHVzZXJzKSl7CiAgICAgICAgdHJ5IHsKICAgICAgICAgICAgY29uc3QgcGF0aCA9IGAke3VzZXJzfVxcJHtlbnRyeS5uYW1lfVxcQXBwRGF0YVxcUm9hbWluZ1xcTWljcm9zb2Z0XFxXaW5kb3dzXFxSZWNlbnRgOwogICAgICAgICAgICBmb3IgKGNvbnN0IGVudHJ5IG9mIERlbm8ucmVhZERpclN5bmMocGF0aCkpewogICAgICAgICAgICAgICAgaWYgKCFlbnRyeS5uYW1lLmVuZHNXaXRoKCJsbmsiKSkgewogICAgICAgICAgICAgICAgICAgIGNvbnRpbnVlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICAgICAgY29uc3QgbG5rX2ZpbGUgPSBgJHtwYXRofVxcJHtlbnRyeS5uYW1lfWA7CiAgICAgICAgICAgICAgICBjb25zdCBsbmsgPSBnZXRMbmtGaWxlKGxua19maWxlKTsKICAgICAgICAgICAgICAgIHJlY2VudF9maWxlcy5wdXNoKGxuayk7CiAgICAgICAgICAgIH0KICAgICAgICB9IGNhdGNoIChfZXJyb3IpIHsKICAgICAgICAgICAgY29udGludWU7CiAgICAgICAgfQogICAgfQogICAgcmV0dXJuIHJlY2VudF9maWxlczsKfQptYWluKCk7Cgo="
```

macOS colleciton focusing on collecting data to help investigate a macOS
incident.

```toml
system = "macos"

[output]
name = "macos_collection"
directory = "./tmp"
format = "jsonl"
compress = true
endpoint_id = "6c51b123-1522-4572-9f2a-0bd5abd81b82"
collection_id = 1
output = "local"

[[artifacts]]
artifact_name = "processes"
[artifacts.processes]
md5 = true
sha1 = false
sha256 = false
metadata = true

[[artifacts]]
artifact_name = "loginitems"

[[artifacts]]
artifact_name = "emond"

[[artifacts]]
artifact_name = "fseventsd"

[[artifacts]]
artifact_name = "launchd"

[[artifacts]]
artifact_name = "files"
[artifacts.files]
start_path = "/"
depth = 90
metadata = true
md5 = true
sha1 = false
sha256 = false
regex_filter = ""

[[artifacts]]
artifact_name = "users"

[[artifacts]]
artifact_name = "groups"

[[artifacts]]
artifact_name = "systeminfo"

[[artifacts]]
artifact_name = "shell_history"

[[artifacts]]
artifact_name = "chromium-history"

[[artifacts]]
artifact_name = "chromium-downloads"

[[artifacts]]
artifact_name = "firefox-history"

[[artifacts]]
artifact_name = "firefox-downloads"

[[artifacts]]
artifact_name = "safari-history"

[[artifacts]]
artifact_name = "safari-downloads"

[[artifacts]]
artifact_name = "cron"

[[artifacts]]
artifact_name = "unifiedlogs"
[artifacts.unifiedlogs]
sources = ["Persist", "Special", "Signpost", "HighVolume"] # Option to specify the log directories (sources)
```
