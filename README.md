# CloudFormation StackSets Setup
Setup CloudTrail for the account

## Notes
This will create the roles necessary to use StackSets to deploy CF across multiple regions in the same account.

## Prerequisites
[AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/)

## Setup roles
Before you can deploy stacksets, you need to deploy roles. The CloudFormation script in this repo creates the necessary roles and permissions to support StackSets.

Using bash, run the following commands
```
aws cloudformation create-stack --region us-east-1 --stack-name aCloudFormationStackSets --template-body file://./cfn-cloudformation-stacksets.yaml --capabilities CAPABILITY_NAMED_IAM
```

## To deploy stacks across all regions for a given account

Using bash, run the following commands
```
# Create the stack set. This will be the container for instances created below. cfn-myscript.yaml is the name of your cloudformation script you want to run everywhere.
aws cloudformation create-stack-set --stack-set-name MyStackSet --template-body file://./cfn-myscript.yaml 

# Be sure to set your account Id. AWS will complain if you don't
TEMP_ACCOUNTID='TheAccountId'

# This grabs all regions and shoves them into a variable. You can, of course, just pick a few regions if you want
TEMP_ALLREGIONS=$(aws ec2 describe-regions --query 'Regions[].{Name:RegionName}' --output text | paste -sd " " -)

# This does the work of creating a stack instace for each region.
aws cloudformation create-stack-instances --stack-set-name MyStackSet --accounts $TEMP_ACCOUNTID --regions $TEMP_ALLREGIONS 
```

## That's it
You can check the status of your stack set via the CLI or the GUI. 

