# stob

Minimal example me using obstore with a custom endpoint. 


```python
import obstore 
from os import environ as env

env["AWS_ENDPOINT_URL"] = "https://projects.pawsey.org.au"

store = obstore.store.from_url("s3://idea-objects", skip_signature = True, region = '')


# Recursively list all files below the 'data' path.
# 1. On AWS S3 this would be the 'data/' prefix
# 2. On a local filesystem, this would be the 'data' directory
prefix = None ##"data"

# Get a stream of metadata objects:
list_stream = obstore.list(store, prefix)

# Print info
for batch in list_stream:
    for meta in batch:
        print(f"Name: {meta["path"]}, size: {meta["size"]}")

```

Another example, list all the cogs in idea-oisst

```python
import obstore 
from os import environ as env

env["AWS_ENDPOINT_URL"] = "https://projects.pawsey.org.au"

store = obstore.store.from_url("s3://idea-oisst", skip_signature = True, region = '')
l = store.list_with_delimiter()
len(l["objects"])
#15973
```

Get the curated object list

```python
import pyarrow.parquet as pq

from obstore.fsspec import FsspecStore
from os import environ as env

env["AWS_ENDPOINT_URL"] = "https://projects.pawsey.org.au"


fs = FsspecStore("s3", skip_signature=True, region="")

url = "s3://idea-objects/idea-curated-objects.parquet"
parquet_file = pq.ParquetFile(url, filesystem=fs)

print(parquet_file.schema_arrow)

```
