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

![infrastructure-overview](images/architecture-overview.png)

The repository consists of a set of nested templates that deploy the following:

 - A tiered [VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html) with public and private subnets, spanning an AWS region.
 - A highly available ECS cluster deployed across two [Availability Zones](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in an [Auto Scaling](https://aws.amazon.com/autoscaling/) group.
 - A Single [NAT gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html) to handle outbound traffic.
 - Two interconnecting microservices deployed as [ECS services](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html) (website-service and product-service). 
 - An [Application Load Balancer (ALB)](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/) to the public subnets to handle inbound traffic.
 - ALB path-based routes for each ECS service to route the inbound traffic to the correct service.
 - Centralized container logging with [Amazon CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html).

## Why use AWS CloudFormation with Amazon ECS?

Using CloudFormation to deploy and manage services with ECS has a number of nice benefits over more traditional methods ([AWS CLI](https://aws.amazon.com/cli), scripting, etc.). 

#### Infrastructure-as-Code

A template can be used repeatedly to create identical copies of the same stack (or to use as a foundation to start a new stack).  Templates are simple YAML- or JSON-formatted text files that can be placed under your normal source control mechanisms, stored in private or public locations such as Amazon S3, and exchanged via email. With CloudFormation, you can see exactly which AWS resources make up a stack. You retain full control and have the ability to modify any of the AWS resources created as part of a stack. 

#### Self-documenting 

Fed up with outdated documentation on your infrastructure or environments? Still keep manual documentation of IP ranges, security group rules, etc.?

With CloudFormation, your template becomes your documentation. Want to see exactly what you have deployed? Just look at your template. If you keep it in source control, then you can also look back at exactly which changes were made and by whom.

#### Intelligent updating & rollback

CloudFormation not only handles the initial deployment of your infrastructure and environments, but it can also manage the whole lifecycle, including future updates. During updates, you have fine-grained control and visibility over how changes are applied, using functionality such as [change sets](https://aws.amazon.com/blogs/aws/new-change-sets-for-aws-cloudformation/), [rolling update policies](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-updatepolicy.html) and [stack policies](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html).

## Template details

The templates below are included in this repository and reference architecture:

| Template | Description |
| --- | --- | 
| [master.yaml](master.yaml) | This is the master template - deploy it to CloudFormation and it includes all of the others automatically. |
| [infrastructure/webapp-vpc.yaml](infrastructure/webapp-vpc.yaml) | This template deploys a VPC with a pair of public and private subnets spread across two Availability Zones. It deploys an [Internet gateway](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html), with a default route on the public subnets. It deploys a pair of NAT gateways (one in each zone), and default routes for them in the private subnets. |
| [infrastructure/webapp-securitygroup.yaml](infrastructure/webapp-securitygroup.yaml) | This template contains the [security groups](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) required by the entire stack. They are created in a separate nested template, so that they can be referenced by all of the other nested templates. |
| [infrastructure/webapp-elb-appserver.yaml](infrastructure/webapp-elb-appserver.yaml) | This template deploys an ALB to the public subnets, which exposes the various ECS services. It is created in in a separate nested template, so that it can be referenced by all of the other nested templates and so that the various ECS services can register with it. |
| [infrastructure/webapp-elb-webserver.yaml](infrastructure/webapp-elb-webserver.yaml) | This template deploys an ALB to the public subnets, which exposes the various ECS services. It is created in in a separate nested template, so that it can be referenced by all of the other nested templates and so that the various ECS services can register with it. |
| [infrastructure/webapp-autoscaling-appserver.yaml](infrastructure/webapp-autoscaling-appserver.yaml) | This template deploys an ECS cluster to the private subnets using an Auto Scaling group. |
| [infrastructure/webapp-autoscaling-webserver.yaml](infrastructure/webapp-autoscaling-webserver.yaml) | This template deploys an ECS cluster to the private subnets using an Auto Scaling group. |
| [infrastructure/webapp-cdn.yaml](infrastructure/webapp-cdn.yaml) | This template deploys an ECS cluster to the private subnets using an Auto Scaling group. |
| [infrastructure/webapp-cloudwatch.yaml](infrastructure/webapp-cloudwatch.yaml) | This template deploys an ECS cluster to the private subnets using an Auto Scaling group. |
| [infrastructure/webapp-iam.yaml](infrastructure/webapp-iam.yaml) | This template deploys an ECS cluster to the private subnets using an Auto Scaling group. |
| [infrastructure/webapp-rds.yaml](infrastructure/webapp-rds.yaml) | This template deploys an ECS cluster to the private subnets using an Auto Scaling group. |
| [infrastructure/webapp-route53.yaml](infrastructure/webapp-route53.yaml) | This template deploys an ECS cluster to the private subnets using an Auto Scaling group. |
| [infrastructure/webapp-s3bucket.yaml](infrastructure/webapp-s3bucket.yaml) | This template deploys an ECS cluster to the private subnets using an Auto Scaling group. |

After the CloudFormation templates have been deployed, the [stack outputs](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/outputs-section-structure.html) contain a link to the load-balanced URLs for each of the deployed microservices.

![stack-outputs](images/stack-outputs.png)
