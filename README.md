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
Index  | Database
Type  | Table
Document | Row
Field | Column

**Node**: A node is a running instance of Elasticsearch.

**Cluster**: a cluster consists of one or more nodes with the same `cluster.name` that are working together to share their data and workload

**Master Node**: One node in the cluster is elected to be the master node. It is in charge of managing cluster-wide changes like creating or deleting an index, or adding or removing a node from the cluster.

---

**Index**: a place to store related data. In reality, an index is just a “logical namespace” which points to one or more physical shards.

**Shard**: A [shard](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/inside-a-shard.html) is a low-level “worker unit” which holds just a slice of all the data in the index. [Read more](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/_add_an_index.html)



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

### Search in all indexes

We can search for documents in all indexes and all types. The following request will return the first 10 documents in all indexes:

```
GET /_search
```

### Search in given indexes and types

We can search for documents in a specific index:

```
GET /{index}/_search
```

Search in `gb` and `us` indexes:

```
GET /gb,us/_search
```

Search all types in any indices beginning with `g` or beginning with `u`

```
GET /g*,u*/_search
```


### Search in index types

Alternatively, we can search for documents of a given type in a specific index:

```
GET /{index}/{type}/_search
```

Search types `user` and `tweet` in the `gb` and `us` indices:

```
GET /gb,us/user,tweet/_search
```

Search types `user` and `tweet` in all indices:

```
GET /_all/user,tweet/_search
```


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

### Parameters

**timeout**: the number of miliseconds used to fetch the data. Alternative syntax: `10`, `10ms` or `1s` for 1 second.

```
GET /_search?timeout=10ms
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
