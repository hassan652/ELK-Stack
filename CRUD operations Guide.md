## Part 1: Intro to Elasticsearch & Kibana.


This repo contains all resources needed in order to get started with Elasticsearch and Kibana.

Main point(s):

- perform CRUD(Create, Read, Update, and Delete) operations with Elasticsearch and Kibana

## Resources

[Free Elastic Cloud Trial](https://ela.st/elastic-beginners)


[Instructions](https://dev.to/elastic/downloading-elasticsearch-and-kibana-macos-linux-and-windows-1mmo) for downloading Elasticsearch and Kibana


[Blog](https://dev.to/lisahjung/beginner-s-guide-to-elasticsearch-4j2k) Beginner's guide to Elasticsearch


## Getting information about cluster and nodes
Syntax: 
```
GET _API/parameter
```
### Example | Get info about cluster health
```
GET _cluster/health
```

### Get info about nodes in a cluster
```
GET _nodes/stats
```

## Performing CRUD operations

## C - Create
### Create an index
Syntax:
```
PUT Name-of-the-Index
```
Example:
```
PUT favorite_candy
```

#### Index a document
When indexing a document, both HTTP verbs `POST` or `PUT` can be used. 

1) Use POST when you want Elasticsearch to autogenerate an id for your document. 

Syntax:
```
POST Name-of-the-Index/_doc
{
  "field": "value"
}
````
Example:
```
POST favorite_candy/_doc
{
  "first_name": "Lisa",
  "candy": "Sour Skittles"
}
```

2) Use PUT when you want to assign a specific id to your document(i.e. if your document has a natural identifier - purchase order number, patient id, & etc).
For more detailed explanation, check out this [documentation](https://www.elastic.co/guide/en/elasticsearch/guide/current/index-doc.html) from Elastic! 

Syntax:
```
PUT Name-of-the-Index/_doc/id-you-want-to-assign-to-this-document
{
  "field": "value"
}
```
Example:
```
PUT favorite_candy/_doc/1
{
  "first_name": "John",
  "candy": "Starburst"
}
```

### _create Endpoint
When you index a document using an id that already exists, the existing document is overwritten by the new document. 
If you do not want a existing document to be overwritten, you can use the _create endpoint! 

With the _create Endpoint, no indexing will occur and you will get a 409 error message. 

Syntax:
```
PUT Name-of-the-Index/_create/id-you-want-to-assign-to-this-document
{
  "field": "value"
}
```
Example:
```
PUT favorite_candy/_create/1
{
  "first_name": "Finn",
  "candy": "Jolly Ranchers"
}
```


## R - READ
### Read a document 
Syntax:
```
GET Name-of-the-Index/_doc/id-of-the-document-you-want-to-retrieve
```
Example:
```
GET favorite_candy/_doc/1
```

## U - UPDATE
### Update a document

IMPORTANT REMINDER: When you index a document using an id that already exists, the existing document is overwritten by the new document. 
If you do not want a existing document to be overwritten, you can use the _create endpoint! 

If you want to update fields in a document, use the following syntax:
```
POST Name-of-the-Index/_update/id-of-the-document-you-want-to-update
{
  "doc": {
    "field1": "value",
    "field2": "value",
  }
} 
```
Example:
```
POST favorite_candy/_update/1
{
  "doc": {
    "candy": "M&M's"
  }
}
```

## D- DELETE
### Delete a document

Syntax:
```
DELETE Name-of-the-Index/_doc/id-of-the-document-you-want-to-delete
```
Example:
```
DELETE favorite_candy/_doc/1
```




