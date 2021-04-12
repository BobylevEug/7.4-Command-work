# 7.4-Command-work

## 2.

```yml
#SERVER.YAML

# repos lists the config for specific repos.
repos:
  # id can either be an exact repo ID or a regex.
  # If using a regex, it must start and end with a slash.
  # Repo ID's are of the form {VCS hostname}/{org}/{repo name}, ex.
  # github.com/runatlantis/atlantis.
- id: /.*/
  # branch is an regex matching pull requests by base branch
  # (the branch the pull request is getting merged into).
  # By default, all branches are matched
  branch: /.*/

  # apply_requirements sets the Apply Requirements for all repos that match.
  apply_requirements: [approved, mergeable]

  # workflow sets the workflow for all repos that match.
  # This workflow must be defined in the workflows section.
  workflow: custom

  # allowed_overrides specifies which keys can be overridden by this repo in
  # its atlantis.yaml file.
  allowed_overrides: [apply_requirements, workflow]

  # allowed_workflows specifies which workflows the repos that match 
  # are allowed to select.
  allowed_workflows: [custom]

  # allow_custom_workflows defines whether this repo can define its own
  # workflows. If false (default), the repo can only use server-side defined
  # workflows.
  allow_custom_workflows: true
  
  # pre_workflow_hooks defines arbitrary list of scripts to execute before workflow execution.
  #pre_workflow_hooks: 
  #  - run: my-pre-workflow-hook-command arg1

  # id can also be an exact match.
- id: github.com/BobrACDC/terra.git

# workflows lists server-side custom workflows
workflows:
  stage:
    plan:
      steps:
      - run: terraform plan
      - init
      - run: terraform apply
```

```yml
#ATLANTIS.YAML

version: 3
automerge: true
projects:
- name: terra
  dir: .
  workspace: terra
  terraform_version: v0.14.7
  autoplan:
    when_modified: ["*.tf", "../modules/**.tf"]
    enabled: true
  apply_requirements: [mergeable, approved]
  workflow: stage prod
workflows
  stage:
    plan:
      steps:
      - run: terraform plan
      - init      
    apply:
      steps:
      - apply
  prod:
    plan:
      steps:
      - run: terraform plan
      - init      
    apply:
      steps:
      - apply
```

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
