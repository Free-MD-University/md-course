# API

the HTTP api allow to make action in your ECK DB.

## insert :

at the insert time ECK will try to do a mapping the fields for the inserted document index . this mean that elastic try to guess the data type of each fields . there is 2 options to define datatype :

-   Auto : ECK try to guess the type
-   Manual : you define the dicument type of the index  
    to do it manually we can use the HTTP API and send the following body :
    the mapping need to have the following format
    ```json
    {
    	"properties": {
    		"FIELDS_NAME": {
    			"type": "TYPE_VALUE(Flattened/nested/float/binary)"
    		}
    	}
    }
    ```

the insert rest api body need to be :

```json
{
	doc: {
		fields: value,
		fields2: value,
	}
}
```

## Delete:

to delete a document ECK need 2 things :

-   the index
-   the id of the document you want to delete

## Get:

to getting a document we use the HTTP api.
with the get api function we can get a document according to the id and the index of a document.
the query need to be is form:

## exist

allow to check if an index exist or if a document id in a specific index exist (only by id).

## update

the update in elastic can be an upsert function too.
to update a document pass in the document body like the insert the id of the document.
you can pass an upsert boolean parameter, if the document id exist ECK will update the value
of the body in the new document. value in the old document not given in the new one are not override.
if the id doesnt exist:

-   the upsert parameter is at true : the docuement is inserted
-   the upsert parameter is at false : the api launch an error

the body of the rest query need to be :

```json
{
	doc: {
		fields: value,
		fields2: value,
	}
	doc_as_upsert: true/false,
}
```

### remove a fields:

to remove a fields in an update we use the script parameter like that :

```json
{
	"script": {
		"source": "ctx._source.remove('FIELDS')"
	}
}
```

## search

/POST {index}/\_search

```json
{
	size: the number of doc to retrieve
	query : {here speicific query filter to filter the doc you want (see query section or not put the fields to retrieve all)}

}
```

### query :

I will here explain basic query filter management but I will do an entire section after that to explain everything .

## count:

get the number of document according to an index + a query (like in search)
