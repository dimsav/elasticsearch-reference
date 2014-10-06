elasticsearch-reference
=======================

## Elastic search vs MySql

Database - Index
Table - Type
Row - Document
Column - Field


## Searching

```
GET /my_index/my_type/_search
{
    "query" : {
        "match" : {
            "my_field_name" : "Joe"
        }
    }
}
```
