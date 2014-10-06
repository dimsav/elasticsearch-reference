elasticsearch-reference
=======================

## Elasticsearch vs MySql

Database - Index

Table - Type

Row - Document

Column - Field


## Searching

Returns documents with `first_name` = "Joe"

```
GET /my_index/my_type/_search
{
    "query" : {
        "match" : {
            "first_name" : "Joe"
        }
    }
}
```
