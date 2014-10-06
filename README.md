elasticsearch-reference
=======================

## Elasticsearch vs Sql

Elasticsearch  | Sql
-------------: | :-------------
Database  | Index
Table  | Type
Row | Document
Column | Field


## Match search

Returns documents where `name` matches "Joe Max". Ex: "Joe Max", "Joe Backer".

```
GET /my_index/my_type/_search
{
    "query" : {
        "match" : {
            "name" : "Joe Max"
        }
    }
}
```

## Phrase search

Phrase search returns matches containing the exact term. Ex: it won't return "Joe Backer".

```
GET /my_index/my_type/_search
{
    "query" : {
        "match_phrase" : {
            "name" : "Joe Max"
        }
    }
}
```

## Filtering

Searches for documents where name matches "Joe" and fiters them by requiring `age` to be greater than 30.

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
                    "name" : "Joe" 
                }
            }
        }
    }
}
```

## Highlighting

Add a new "highlight" parameter containing the field names you wish to highlight. [Read more](http://www.elasticsearch.org/guide/en/elasticsearch/reference/1.4/search-request-highlighting.html)

```
GET /my_index/my_type/_search
{
    "query" : {
        "match" : {
            "name" : "Joe Max"
        }
    },
    "highlight": {
        "fields" : {
            "name" : {}
        }
    }
}
```
