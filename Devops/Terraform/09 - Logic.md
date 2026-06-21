# condition :

we can use a condition to define a parameter 

ressource "aws_vpc" "MyVpc"{
    cidr_block = var.env == "dev" ? "10.0.0.0/16" : "8.0.0.0/16"
}

only teriaire are allowed . 


# dynamic 

You can dynamically construct repeatable nested blocks like setting using a special dynamic block type, which is supported inside resource, data, provider, and provisioner blocks:
```tf
resource "aws_elastic_beanstalk_environment" "tfenvtest" {
  name                = "tf-test-name"
  application         = aws_elastic_beanstalk_application.tftest.name
  solution_stack_name = "64bit Amazon Linux 2018.03 v2.11.4 running Go 1.12.6"

  dynamic "setting" {
    for_each = var.settings
    content {
      namespace = setting.value["namespace"]
      name = setting.value["name"]
      value = setting.value["value"]
    }
  }
}
```
A dynamic block acts much like a for expression, but produces nested blocks instead of a complex typed value. It iterates over a given complex value, and generates a nested block for each element of that complex value.