when create a teraform with tf file the terraform create a state file . 
this statefile sav all the data to your state file .
when create a new state the TF will compare both of them and change what need . 

this state file contains sensitive data . 

to use the file with security with keep this file in another s3 or blob storage. 
when use tf command TF will check the current directory by default but you can change to check a remote file . 

To define a terraform backend file : 
terraform {
  required_version = ">= 1.0"
  
  backend "TYPE" {
    PARAMS
  }

  BACKEND "S3" {
    bucket = "bucket name" 
    key = ""
    region = ""
    encrypt = true
    lock_table = ""
  }
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}