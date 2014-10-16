## Debugging errors

Suppose you run the following query but get an error:

```
GET /{index}/{type}/_search
{
	"query":  { "match_one": { "title": "hey dude" }}
}
```

To see the error message do:

```
GET /{index}/{type}/_validate/query?explain
{
    "query":  { "match_one": { "title": "hey dude" }}
}
```