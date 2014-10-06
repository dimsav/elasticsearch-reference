elasticsearch-reference
=======================

* [Terminology](#terminology)
* [Storing](#storing-documents)
* [Retrieving](#retrieving-documents)
* [Searching](#searching)

## Terminology

Elasticsearch  | Sql
-------------: | :-------------
Database  | Index
Table  | Type
Row | Document
Column | Field

## Storing documents

The action of saving documents to elasticsearch, is called "indexing".

### Indexing a document with known id

To index a document with specific id, we use the `PUT` verb. If this id is not already indexed, a new document will be created. If the id is already indexed, the document will be overwritten.

```
PUT /{index}/{type}/{id}
{
  "field": "value",
  ...
}
```

### Indexing a document with auto-generated id

We can also let elasticsearch to auto-generate the id for the indexed document. To do that, we use the verb `POST` and we skip the id in the query:

```
POST /{index}/{type}/
{
  "field": "value",
  ...
}
```

## Retrieving documents

To get a document by its id, we use the `GET` verb:

```
GET /{index}/{type}/{id}
```

## Searching

### Match search

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

### Phrase search

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

### Filtering

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

### Highlighting results

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
