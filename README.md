# Deploying Web Application Architecture with Amazon Web Services

This reference architecture provides a set of YAML templates for deploying the following AWS services :
 - Amazon IAM
 - Amazon VPC
 - Amazon EC2
 - Amazon ELB
 - Amazon AutoScaling
 - Amazon CloudFront
 - Amazon RDS
 - Amazon S3
 - Amazon Cloudwatch
 - Amazon Route53 
 - Amazon Security Group & NACL

## Prerequisites Notes
The Cloudformation Security Group IP address is open by default (testing purpose). You should update the Security Group Access with your own IP Address to ensure your instances security.

Before you can deploy this process, you need the following:
 - Your AWS account must have one VPC available to be created in the selected region
 - Amazon EC2 key pair
 - Installed Domain in Route 53.
 - Installed Certificate (in your selected region & also one in us-east-1) 

## You can launch this CloudFormation stack in the following Region in your account:
 - US East (N. Virginia)
 - US East (Ohio)
 - US West (N. California)
 - US West (Oregon)
 - Asia Pacific (Tokyo)
 - Asia Pacific (Singapore)
 - Asia Pacific (Sydney)


