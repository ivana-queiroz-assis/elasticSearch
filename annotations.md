# Elastic search

## Creating and deleting indices:

DELETE /pages

PUT /products 
{
   "settings"{
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


## 
