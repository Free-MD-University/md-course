# Data Type:

## Basic DT

there is some common data type :

-   float
-   binary
-   int
-   base64
-   boolean

## JSON

but there is also some particular datatype like JSON :
in elastic you can send in a fields a other json object like :

```json
{
	person: {
		name: text,
		age: int,
		lastName: text
	}
}
```

the type will be in the same format of the JSON Object .

### Object

if you use standard JSON object elastic will try to do an indexation, that mean that each inner fields will be like a real fields where the name is the path to the property separated by . :

here we will have :
person.name: text
person.age: int
person.lastName: text

### Flattened Object :

the json is conserved as one entity. one fields is created.
basic search is possible but not special one like range or substring.
we can search the fields and each sub value will be analyzed or search in a special subfields.
is a good options for too nested/big object.

### Nested Object :

## Text Type

### Text:

#### type definition:

```json
{
	"properties": {
		"location": "text"
	}
}
```

#### insert :

used for full text content, at the insert time an analyzer will try to optimized the text for search.

### Completion :

### Search as you type:

### Annotated Text :

## Spatial Data Type:

### GeoPoint:

#### type definition:

```json
{
	"properties": {
		"location": "geo_point"
	}
}
```

#### insert :

this is to define latitude/logitude . need to put the value like :
[lat, long] in an array:

```json
	{
		location: {
			"type": "Point",
			"coordinates": [lat,long]
		}
	}
```

### GeoShape:

#### type definition:

```json
{
	"properties": {
		"location": "geo_shape"
	}
}
```

#### insert :

this is to define a spape with multiple place. need to put the value like :

```json
	{
		location: {
			"type": "LineString", // or other shape type
			"coordinates": [[lat,long], [lat,long], [lat,long]]
		}
	}
```

### Point:

same as GeoPoint same insert just the datatype definition is different need to be _point_ not geo_point

### Shape:

same as GeoShape same insert just the datatype definition is different need to be _shape_ not geo_shape
