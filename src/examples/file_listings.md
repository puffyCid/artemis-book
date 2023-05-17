# File Listings

Windows TOML collection looking for all files created in the last 14 days

```toml
system = "windows"

[output]
name = "recent_files"
directory = "./tmp"
format = "jsonl"
compress = false
endpoint_id = "abdc"
collection_id = 1
output = "local"
filter_name = "recently_created_files"
# This script will take the search artifact below and filter it to only return files that were created in the past 14 days
filter_script = "Ly8gbWFpbi50cwpmdW5jdGlvbiBtYWluKCkgewogIGNvbnN0IGFyZ3MgPSBEZW5vLmFyZ3M7CiAgaWYgKGFyZ3MubGVuZ3RoID09PSAwKSB7CiAgICByZXR1cm4gW107CiAgfQogIGNvbnN0IGRhdGEgPSBKU09OLnBhcnNlKGFyZ3NbMF0pOwogIGNvbnN0IHRpbWVfbm93ID0gbmV3IERhdGUoKTsKICBjb25zdCBtaWxsaXNlY29uZHMgPSB0aW1lX25vdy5nZXRUaW1lKCk7CiAgY29uc3Qgc2Vjb25kcyA9IG1pbGxpc2Vjb25kcyAvIDFlMzsKICBjb25zdCBmb3VydGVlbl9kYXlzID0gMTIwOTYwMDsKICBjb25zdCBlYXJsaWVzdF9zdGFydCA9IHNlY29uZHMgLSBmb3VydGVlbl9kYXlzOwogIGNvbnN0IGZpbHRlcl9kYXRhID0gW107CiAgZm9yIChjb25zdCBlbnRyeSBvZiBkYXRhKSB7CiAgICBpZiAoZW50cnkuY3JlYXRlZCA8IGVhcmxpZXN0X3N0YXJ0KSB7CiAgICAgIGNvbnRpbnVlOwogICAgfQogICAgZmlsdGVyX2RhdGEucHVzaChlbnRyeSk7CiAgfQogIHJldHVybiBmaWx0ZXJfZGF0YTsKfQptYWluKCk7Cg=="

[[artifacts]]
artifact_name = "files" # Name of artifact
filter = true
[artifacts.files]
start_path = "C:\\" # Start of file listing
depth = 100 # How many sub directories to descend
```

macOS TOML collection looking for all files created in the last 14 days

```toml
system = "macos"

[output]
name = "recent_files"
directory = "./tmp"
format = "jsonl"
compress = false
endpoint_id = "abdc"
collection_id = 1
output = "local"
filter_name = "recently_created_files"
# This script will take the search artifact below and filter it to only return files that were created in the past 14 days
filter_script = "Ly8gbWFpbi50cwpmdW5jdGlvbiBtYWluKCkgewogIGNvbnN0IGFyZ3MgPSBEZW5vLmFyZ3M7CiAgaWYgKGFyZ3MubGVuZ3RoID09PSAwKSB7CiAgICByZXR1cm4gW107CiAgfQogIGNvbnN0IGRhdGEgPSBKU09OLnBhcnNlKGFyZ3NbMF0pOwogIGNvbnN0IHRpbWVfbm93ID0gbmV3IERhdGUoKTsKICBjb25zdCBtaWxsaXNlY29uZHMgPSB0aW1lX25vdy5nZXRUaW1lKCk7CiAgY29uc3Qgc2Vjb25kcyA9IG1pbGxpc2Vjb25kcyAvIDFlMzsKICBjb25zdCBmb3VydGVlbl9kYXlzID0gMTIwOTYwMDsKICBjb25zdCBlYXJsaWVzdF9zdGFydCA9IHNlY29uZHMgLSBmb3VydGVlbl9kYXlzOwogIGNvbnN0IGZpbHRlcl9kYXRhID0gW107CiAgZm9yIChjb25zdCBlbnRyeSBvZiBkYXRhKSB7CiAgICBpZiAoZW50cnkuY3JlYXRlZCA8IGVhcmxpZXN0X3N0YXJ0KSB7CiAgICAgIGNvbnRpbnVlOwogICAgfQogICAgZmlsdGVyX2RhdGEucHVzaChlbnRyeSk7CiAgfQogIHJldHVybiBmaWx0ZXJfZGF0YTsKfQptYWluKCk7Cg=="

[[artifacts]]
artifact_name = "files" # Name of artifact
filter = true
[artifacts.files]
start_path = "/" # Start of file listing
depth = 100 # How many sub directories to descend
```
