# Week 1

## 1. Data Lakes

* Centralized and secure repository
* Store, discover and share data at any scale
    * Structured (CSV, TSV)
    * Semi-structured (JSON, XML)
    * Unstructured (images, audio)
    * Streaming data
* Built on top of object storage
    * Amazon Simple Storage Service (S3)
    * Object storage - data managed and stored as objects
    * Contains data, metadata, unique identifier


## 2. AWS Data Wrangler

* Open source Python library
* Connects Pandas DataFrames and AWS data services
* Load/unload data from
    * data lakes
    * databases
    * data warehouses

```py
!pip install awswrangler

import awswrangler as wr
import pandas as pd

# Retrieve the data directly from Amazon S3
df = wr.s3.read_csv(path="s3://bucket/prefix/")
```

## 3. AWS Glue Data Catalogue

* Creates reference to data ("S3-to-table" mapping)
* 