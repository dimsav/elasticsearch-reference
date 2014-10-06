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

## Filtering

Fiters the above results by requiring `age` to be greater than 30.

```
GET /megacorp/employee/_search
{
   "query" : {
        "filtered" : {
            "filter" : {
                "range" : {
                    "age" : { "gt" : 30 } 
                }
            },
            "query" : {
                "match" : {
                    "first_name" : "Joe" 
                }
            }
        }
    }
}
```
