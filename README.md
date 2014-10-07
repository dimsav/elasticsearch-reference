elasticsearch-reference
=======================

* [Terminology](#terminology)
* [Storing](#storing-documents)
* [Retrieving](#retrieving-documents)
* [Deleting](#deleting-documents)
* [Searching](#searching)
* [Health](#cluster-health)

## Terminology

Elasticsearch  | Sql
-------------: | :-------------
Database  | Index
Table  | Type
Row | Document
Column | Field

**Node**: A node is a running instance of Elasticsearch.

**Cluster**: a cluster consists of one or more nodes with the same `cluster.name` that are working together to share their data and workload

**Master Node**: One node in the cluster is elected to be the master node. It is in charge of managing cluster-wide changes like creating or deleting an index, or adding or removing a node from the cluster.

## Storing documents

The action of saving documents to elasticsearch, is called "indexing".  [See more](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/index-doc.html)

### Indexing a document with known id

To index a document with specific id, we use the `PUT` verb. If this id is not already indexed, a new document will be created. If the id is already indexed, the whole document will be overwritten.

```
PUT /{index}/{type}/{id}
{
  "field": "value",
  ...
}
```

### Indexing a document with auto-generated id

We can also let elasticsearch to auto-generate the id for the indexed document. To do that, we use the verb `POST` and we skip the id in the query.

```
POST /{index}/{type}/
{
  "field": "value",
  ...
}
```

## Retrieving documents

To get a document by its id, we use the `GET` verb. [See more](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/get-doc.html)

```
GET /{index}/{type}/{id}
```

To check if the document exists, check the status code of the response. A code `404 Not Found` will be returned if not.

## Deleting documents

This is the syntax to delete a document with known id. Same as before, different status codes will be returned if the delete was succesfull or not. [Read more](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/delete-doc.html)

```
DELETE /{index}/{type}/{id}
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

## Cluster health

To get the status of the system do:

```
GET /_cluster/health
```

The response will be something like: 

```
{
   "cluster_name":          "elasticsearch",
   "status":                "green", 
   "timed_out":             false,
   "number_of_nodes":       1,
   "number_of_data_nodes":  1,
   "active_primary_shards": 0,
   "active_shards":         0,
   "relocating_shards":     0,
   "initializing_shards":   0,
   "unassigned_shards":     0
}
```

### Status:

**green**: All primary and replica shards are active.

**yellow**: All primary shards are active, but not all replica shards are active.

**red**: Not all primary shards are active.
