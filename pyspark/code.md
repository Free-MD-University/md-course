# read data 

to read data we use the spark context

```python
spark.read.format('{format}').option('{option name}', VALUE).schema('{string schema}').option().load('{file path}')
```

options importante : 

- inferSchema: find the schema from the data (not work with schema function)
- multiline: for json is the json is in multiline 
- header: is header is present in the csv

the data read will return a DF of all the data .

# Schema
see the current schma of the data : 
```
df.printSchema() 
```


## define schema: 

### String
```
schema = '''
    COLUMN TYPE,
    COLUMN TYPE
'''
```

### Python object

StructType([
    StructField({fieldName}, PysparkType, True)
])



# Show data

## show 
basic output

df.show()

## display 
more accurate output .

df.display()

# Select 

when we have to create a DF with only some filds we can use select

```
df.select('column1','column2')
df.select(col('column1'),col('column2'))
```

# Alias
use for give a new name to a column 

```
df.select(col('column1').alias('new name'))

```

# Filter

## ==
```python
df.filter(col('column1')==VALUE)
```

## OR / AND
```python
df.filter((col('column1')==VALUE & col('column2')==VALUE))
df.filter((col('column1')==VALUE | col('column2')==VALUE))
```

## Null
```python
df.filter(col('column1').isNull())
```

## Not

## isin
```python
df.filter(col('column1').isin([VALUE!],[VALUE2]))
```

# withColumn
create a new column with new values
```python
df.withColumn('new col name',COLUMN VALUE)
df.withColumn('new col name',col(1) * col(2))
```

# transfo

## cast

```
df.withColumn('new col name',col(1).cast(newType))
```

## string

### UPPER

### LOWER

### INITCAP
make upper each first letter work 
```python
df.select(initcap('{column name}'))
```

## date

### CURRENT_DATE()
```
df.withColumn('new date',current_date())
```


### DATE_ADD
```
df.withColumn('new date',date_add('date',days to add))
```

### DATE_SUB()

### DATE_DIFF


## List

### EXPLODE 
take a column that is a list and split it 

### SPLIT 
take a string split it as a list .

# Sort

sort take or ascending or descending argument is an array on col to sort with 0 for descending this column and 1 for sort ascending
the sort order is important as SQL . the second col is used only when the first is equal

```
df.select(col('column1'), col(col2)).sort([col1, col2], ascending=[0,1])
```

# limit
limit as sql

```
df.select(col('column1')).limit(number to limit)
```

# Drop

drop a column from a df

```
df.drop('col name', 'col 2')
```

## dropna

drop row with null value in one column
```
df.dropna()
```

of for null value in specific column
```
df.dropna(subset=['{col name}'])
```


# Drop Duplicate
drop duplicate row values on all column (all column need to be equal to be considered as duplicated)
```
df.dropDuplicates()
```

drop on subset column (all subset column need to be equal to be considered as duplicated)
```
df.dropDuplicates(subsets=['col1','col2'])
```

# disntint
like drop duplicate but on a specific fieds not on the all row

# Union

## Union  

can union two dataframe with same column ORDER in one (not merge column add rows together )

```python
df1.union(df2)
```

if the dataframe have the same column name but not same order

```python
df1.unionByColumnName(df2)



# 