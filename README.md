# CloudFormation StackSets Setup
Setup CloudTrail for the account

## Notes
This will create the roles necessary to use StackSets to deploy CF across multiple regions in the same account.

## Prerequisites
[AWS CLI](http://docs.aws.amazon.com/rekognition/latest/dg/setup-awscli.html)

## Setup
By convention, scripts like this that are deployed account-wide are deployed to us-east-1 (N. Virginia)

```
aws cloudformation create-stack --region us-east-1 --stack-name aCloudFormationStackSets --template-body file://./cfn-cloudformation-stacksets.yaml --capabilities CAPABILITY_NAMED_IAM
```