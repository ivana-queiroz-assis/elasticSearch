# Elastic search - Managing documents

## Creating and deleting indices:

DELETE /pages

PUT /products 
{
   "settings":{
      "number_of_shards": 2,
      "number_of_replicass":2
   }
}

## Retrieving documents by ID:

GET /produts/_doc/100


## Updating documents:

POST /produts/_update/100
{
  "doc": {
    "in_stock": 2
  }
}

POST /produts/_update/100
{
  "doc": {
    "tags":[
      "eletronics",
      "videogame"
    ]
  }
}

## Scripted updates( accept IF Statements)

POST /produts/_update/100
{
  "script": {
    "source": "ctx._source.in_stock--"
  }
}

POST /produts/_update/100
{
  "script": {
    "source": "ctx._source.in_stock=10"
  }
}

POST /produts/_update/100
{
  "script": {
    "source": "ctx._source.in_stock-=params.quantity",
    "params": {
      "quantity": 4
    }
  }
}

GET /produts/_doc/100

## Upsert: cria o objeto caso não exista, executa o script caso exista.

POST /produts/_update/101
{
  "script": {
    "source": "ctx._source.in_stock++"
  },
  "upsert": {
    "name": "Chocolate",
    "price":10,
    "in_stock":5
  }
}

GET /produts/_doc/101

## Replacing documents:

PUT /produts/_doc/100
{
  "nome" : "Toaster",
    "price" : 400,
    "in_stock" : 60
}

GET /produts/_doc/100


## Delete documents:

DELETE /produts/_doc/101

GET /produts/_doc/101

### Update by query ( updating multiple objects)
POST /produts/_update_by_query
{
 "script": {
    "source": "ctx._source.price--"
  },
  "query":{
    "match_all":{  }
  }
}

### Recuperando vários itens por query
GET /produts/_search
{
  "query": {
    "match_all": {}
  }
}

Response:

{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "201",
        "_score" : 1.0,
        "_source" : {
          "name" : "Milk Frother",
          "price" : 129,
          "in_stock" : 14
        }
      }
    ]
  }
}

### Deletando vários itens por query
POST /produts/_delete_by_query
{
  "query":{
    "match_all":{}
  }
}

### Bulk API 

Perfom actiona(index, update, delete ) in many documents with single query. Use NDJSON format.

POST /_bulk
{"index":{ "_index": "products", "_id": 200  }}
{"name": "Expresso Machine", "price": 199, "in_stock":5 }
{"create": {"_index": "products", "_id": 201}}
{"name": "Milk Frother", "price": 149, "in_stock": 14}


POST /products/_bulk
{"update": {"_id": 201}}
{"doc": {"price": 129}}
{"delete": {"_id": 200}}

Response:

{
  "took" : 23,
  "errors" : false,
  "items" : [
    {
      "update" : {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "201",
        "_version" : 2,
        "result" : "updated",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 2,
        "_primary_term" : 1,
        "status" : 200
      }
    },
    {
      "delete" : {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "200",
        "_version" : 2,
        "result" : "deleted",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 3,
        "_primary_term" : 1,
        "status" : 200
      }
    }
  ]
}

### Curl para usar o BULK 

curl -H "Content-Type: application/x-ndjson" -XPOST http://localhost:9200/products/_bulk --data-binary "@products.json" 

 
