# Remote Uploads

`artemis` has basic support for uploading collections to three (3) external
cloud services:

1. Google Cloud Platform (GCP)
2. Microsoft Azure
3. Amazon Web Services (AWS)

Uploading collections to a remote serivce requires three (3) steps:

1. Name of remote service. Valid options are: `"gcp", "azure", "aws"`
2. URL to the remote service
3. A base64 encoded API key formatted based on the remote service select in
   step 1.

An example TOML Collection is below:

```toml
system = "windows"

[output]
name = "shimcache_collection"
directory = "./tmp"
format = "json"
compress = true
endpoint_id = "6c51b123-1522-4572-9f2a-0bd5abd81b82"
collection_id = 1
output = "gcp"
url = "URLHERE"
api_key = "base64EncodedHere"

[[artifacts]]
artifact_name = "shimcache"
[artifacts.shimcache]
```

# GCP

(Explain GCP process)

# Azure

(Explain Azure process)

# AWS

(Explain AWS process)
