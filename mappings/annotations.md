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
