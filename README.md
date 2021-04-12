# 7.4-Command-work

## 1.

## 2.

## 3.
```yml
terraform {
   backend "s3" {
    bucket = "netol-test"
    key    = "test/terraform.tfstate"
    region = "us-west-2"
  }
}
module "ec2-instance" {
  source                 = "terraform-aws-modules/ec2-instance/aws"
  version                = "~> 2.0"

  name                   = "test"
  instance_count         = 1

  ami                    = "ami-ebd02392"
  instance_type          = "t2.micro"
  monitoring             = true
  subnet_id              = "subnet-eddcdzz4"
 
}


data "aws_caller_identity" "current" {}'
```
