the main is the main file read by terraform.


it defined like : 

```terraform 
terraform {
    required_providers {
        PROV = {
            source = 
            version =
        }
    }
}

provider "PROV" {
    define variable of terraform provider
}

FOLLOWING
```


```
provider "aws" {
  region = var.aws_region
}

terraform {
  required_version = ">= 1.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

```

