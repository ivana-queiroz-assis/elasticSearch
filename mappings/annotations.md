# Elastic search - Mapping and analysis

## Analyzers

### Character filters

Add, removes or changes characters. 
 
 Ex.: remove html tags

### Tokenizer

Tokenizes a string, splits it to tokens.

Ex.: Input: "Je veux mange quelque chose!"
     Output: "Je","veux","mange","quelque","chose"

### Token filters

Receive the output of the tokenizer as input. Can add, remove or modify tokens.

Ex.: lowercase filter. 

## API Analyze

API to analize the data. It's use for the your own analyzers.

POST /_analyze
{
  "text": "Quelles sont les deux plus vieilles lettres de l’alphabet? C’est clair: A, G",
  "analyzer": "standard"
}

Response:

{
  "tokens" : [
    {
      "token" : "quelles",
      "start_offset" : 0,
      "end_offset" : 7,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "sont",
      "start_offset" : 8,
      "end_offset" : 12,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "les",
      "start_offset" : 13,
      "end_offset" : 16,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "deux",
      "start_offset" : 17,
      "end_offset" : 21,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "plus",
      "start_offset" : 22,
      "end_offset" : 26,
      "type" : "<ALPHANUM>",
      "position" : 4
    },
    {
      "token" : "vieilles",
      "start_offset" : 27,
      "end_offset" : 35,
      "type" : "<ALPHANUM>",
      "position" : 5
    },
    {
      "token" : "lettres",
      "start_offset" : 36,
      "end_offset" : 43,
      "type" : "<ALPHANUM>",
      "position" : 6
    },
    {
      "token" : "de",
      "start_offset" : 44,
      "end_offset" : 46,
      "type" : "<ALPHANUM>",
      "position" : 7
    },
    {
      "token" : "l’alphabet",
      "start_offset" : 47,
      "end_offset" : 57,
      "type" : "<ALPHANUM>",
      "position" : 8
    },
    {
      "token" : "c’est",
      "start_offset" : 59,
      "end_offset" : 64,
      "type" : "<ALPHANUM>",
      "position" : 9
    },
    {
      "token" : "clair",
      "start_offset" : 65,
      "end_offset" : 70,
      "type" : "<ALPHANUM>",
      "position" : 10
    },
    {
      "token" : "a",
      "start_offset" : 72,
      "end_offset" : 73,
      "type" : "<ALPHANUM>",
      "position" : 11
    },
    {
      "token" : "g",
      "start_offset" : 75,
      "end_offset" : 76,
      "type" : "<ALPHANUM>",
      "position" : 12
    }
  ]
}

## Default analyzer

POST /_analyze
{
  "text": "Quelles sont les deux plus vieilles lettres de l’alphabet? C’est clair: A, G",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}


### Importants things

One inverted index is create for each field. Because each field has a type ( numeric, date, string) that has differents ways of search.

Keyword data type is use to search the exact matching values. Ex.: PUBLISHED.
Text data type is use for full-text searches.

### Coersion

Inspect mapping and coerce into field data type if possible.

Convert string into, for example.

### Date

PUT /reviews/_doc/2
{
  "rating":4.5,
  "content":"Not bad",
  "product_id":123,
  "created_at": "2015-03-27",
  "author":{
    "first_name": "Joe",
    "last_name": "Average",
    "email": "hsjhsj@example.com"
  }
}

PUT /reviews/_doc/3
{
  "rating":4.5,
  "content":"Not bad",
  "product_id":123,
  "created_at": "2015-04-27T13:07:41Z",
  "author":{
    "first_name": "Joe",
    "last_name": "Average",
    "email": "hsjhsj@example.com"
  }
}

PUT /reviews/_doc/4
{
  "rating":4.5,
  "content":"Not bad",
  "product_id":123,
  "created_at": "2015-01-27T09:21:51+01:00",
  "author":{
    "first_name": "Joe",
    "last_name": "Average",
    "email": "hsjhsj@example.com"
  }
}

PUT /reviews/_doc/5
{
  "rating":4.5,
  "content":"Not bad",
  "product_id":123,
  "created_at": "1589663632426",
  "author":{
    "first_name": "Joe",
    "last_name": "Average",
    "email": "hsjhsj@example.com"
  }
}

GET /reviews/_search
{
  "query":{
    "match_all": {}
  }
}


Response:

{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "reviews",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "rating" : 4.5,
          "content" : "Not bad",
          "product_id" : 123,
          "created_at" : "2015-03-27",
          "author" : {
            "first_name" : "Joe",
            "last_name" : "Average",
            "email" : "hsjhsj@example.com"
          }
        }
      },
      {
        "_index" : "reviews",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "rating" : 4.5,
          "content" : "Not bad",
          "product_id" : 123,
          "created_at" : "2015-04-27T13:07:41Z",
          "author" : {
            "first_name" : "Joe",
            "last_name" : "Average",
            "email" : "hsjhsj@example.com"
          }
        }
      },
      {
        "_index" : "reviews",
        "_type" : "_doc",
        "_id" : "4",
        "_score" : 1.0,
        "_source" : {
          "rating" : 4.5,
          "content" : "Not bad",
          "product_id" : 123,
          "created_at" : "2015-01-27T09:21:51+01:00",
          "author" : {
            "first_name" : "Joe",
            "last_name" : "Average",
            "email" : "hsjhsj@example.com"
          }
        }
      },
      {
        "_index" : "reviews",
        "_type" : "_doc",
        "_id" : "5",
        "_score" : 1.0,
        "_source" : {
          "rating" : 4.5,
          "content" : "Not bad",
          "product_id" : 123,
          "created_at" : "1589663632426",
          "author" : {
            "first_name" : "Joe",
            "last_name" : "Average",
            "email" : "hsjhsj@example.com"
          }
        }
      }
    ]
  }
}
 ### Parameters:
 
  * Format
  * Properties: nested objects
  * Coerce ( enable by default). Could be at index level and attribute level.
  * doc_values: disable invexted index in the attriubute, then reduce the disk space.
  * Norms: used to rank, disable if you don't need to rank by the attribute.
  * Index: disable indexing for a field.
  * null_value: null cannot be indexed or searched 
  * copy_to: used to copy multiple field values into a group field. Ex.: first_name and last_name -> full_name
  
 ### Keyword vs Text
 
 Keyword can be search only passing the exact value and can be sorted and is not indexing.
  
 Text is indexing and have full search and can not be sorted.


### Udpdating existing mappings

Cannot be changed. THe solution is to reindex into a new index.

PUT /reviews_new
{
  "mappings" : {
      "properties" : {
        "author" : {
          "properties" : {
            "email" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "first_name" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "last_name" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            }
          }
        },
        "content" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "created_at" : {
          "type" : "date"
        },
        "product_id" : {
          "type" : "keyword"
        },
        "rating" : {
          "type" : "float"
        }
      }
    }
}

GET /reviews/_mapping

### Reindexing


POST /_reindex
{
  
  "source": {
    "index": "reviews"
  }
  
  , "dest": {"index": "reviews_new"}
}


### Reindexing with script

POST /_reindex
{
  
  "source": {
    "index": "reviews"
  }
  
  , "dest": {"index": "reviews_new"},
  "script": {
    "source": """
      if(ctx._source.product_id != null){
        ctx._source.product_id = ctx._source.product_id.toString();
      }
     """
  }
}

### Deleting the index

POST /reviews_new/_delete_by_query
{
  "query": {"match_all": {}}
}


### Field alias

Is used to query by other field.

Creating a field alias 'comment' targeting content.

PUT /reviews_new/_mapping
{
  "properties":{
    "comment":{
      "type": "alias",
      "path": "content"
    }
  }
}

The followings search queries give the same result:

GET /reviews_new/_search
{
  "query": {
    "match": {
      "content": "Not bad"
     }
    
  }
}


GET /reviews_new/_search
{
  "query": {
    "match": {
      "comment": "Not bad"
     }
    
  }
}

### Multifields
 
You can define that the same field is text and keyword in the same time.

PUT /multi_field_test
{
  "mappings": {
    "properties": {
      "description": {
        "type": "text"
      },
      "ingredients": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword"
          }
        }
      }
    }
  }
}

#### Querying the text mapping:

GET /multi_field_test/_search
{
  "query": {
    "match": {
      "ingredients": "Spaghetti"
    }
  }
}

#### Quering the keyword mapping:

GET /multi_field_test/_search
{
  "query": {
    "term": {
      "ingredients.keyword": "Spaghetti"
    }
  }
}

### Index templates

Example of log template:

PUT /_template/access-logs
{
  "index_patterns": ["access-logs-*"],
  "settings": {
    "number_of_shards": 2,
    "index.mapping.coerce": false
  }, 
  "mappings": {
    "properties": {
      "@timestamp": {
        "type": "date"
      },
      "url.original": {
        "type": "keyword"
      },
      "http.request.referrer": {
        "type": "keyword"
      },
      "http.response.status_code": {
        "type": "long"
      }
    }
  }
}

PUT /access-logs-2020-01-01







 
  