# index: 

we can use a counter with an idenx to create ressource. 
If we create a meta variable named COUNT in a ressource it will loop inside the ressource from 0 to the count. 
to get the number of the loop we use count.index 


```
variable S3Name {
    type = list(str)
    value = ["name1", "name2", "name3"]
}

ressource "aws_s3_bucket" s3 {
    count = 2 (go from 0 to 2)
    bucket = var.S3Name[count.index]
}

```
it will create 3 bucket 

# lenght 
we can use lenght to return the number element of a bucket.
```
variable S3Name {
    type = list(str)
    value = ["name1", "name2", "name3"]
}

ressource "aws_s3_bucket" s3 {
    count = lenght(var.S3Name)
    bucket = var.S3Name[count.index]
}

```
# for_each 
to use a counter in a set we need to use for_each. for each return a key and a value


```
variable S3Name {
    type = set(str)
    value = ("name1", "name2", "name3")
}

ressource "aws_s3_bucket" s3 {
    for_each = var.S3Name
    bucket = each.value
}

```

for each can be used with map also 

# depends on 
allow to define the order of ressource creation 

```
variable S3Name {
    type = set(str)
    value = ("name1", "name2", "name3")
}

ressource "aws_s3_bucket" s3 {
    for_each = var.S3Name
    bucket = each.value
}

ressource "aws_s3_bucket" s32 {
    for_each = var.S3Name
    bucket = each.value
    depends_on = ["s3"]
}

```