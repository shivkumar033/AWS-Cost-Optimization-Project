# AWS Cost Optimization Projects

A hands-on portfolio of AWS projects focused on reducing cloud infrastructure costs while maintaining performance, availability, and security.

## What Is AWS Cost Optimization?

AWS Cost Optimization is the process of managing and reducing unnecessary AWS spending by selecting appropriately sized resources, eliminating idle capacity, improving resource utilization, choosing suitable pricing and storage options, and monitoring usage.

The goal is to avoid waste and get the required business value from every AWS resource without compromising reliability, performance, or security.

## Projects

| # | Project | Description | Technologies | Repository |
|---|---|---|---|---|
| 1 | EC2 Cost Optimization with Lambda | Automatically start and stop selected EC2 instances on a schedule to reduce compute costs during non-working hours. | Amazon EC2, AWS Lambda, EventBridge Scheduler, IAM, Python (boto3) | [View Project](https://github.com/shivkumar033/AWS-Cost-Optimization-Project/blob/main/Project-1/Automatically%20Stop%20and%20Start%20EC2%20Instances.md) |
| 2 | S3 Storage Cost Optimization | Configure S3 Lifecycle rules to transition older objects to lower-cost storage classes, expire eligible data, and abort incomplete multipart uploads. | Amazon S3, Lifecycle Policies, S3 Standard-IA, S3 Glacier | [View Project](https://github.com/YOUR-USERNAME/YOUR-S3-COST-OPTIMIZATION-REPO) |
| 3 | AWS Billing and Budget Alerts | Create a monthly AWS budget and configure alerts for actual or forecasted spending using email notifications. | AWS Budgets, AWS Billing, Amazon SNS, Cost Explorer | [View Project](https://github.com/YOUR-USERNAME/YOUR-BILLING-ALERT-REPO) |
| 4 | Serverless Application Cost Optimization | Deploy and measure a serverless application, then optimize Lambda execution duration, memory, database capacity, API caching, and log retention. | AWS Lambda, API Gateway, DynamoDB, CloudWatch | [View Project](https://github.com/YOUR-USERNAME/YOUR-SERVERLESS-COST-OPTIMIZATION-REPO) |
| 5 | Amazon EKS Cost Optimization | Improve Kubernetes resource utilization with appropriate pod requests and limits, autoscaling, and right-sized node capacity. | Amazon EKS, Kubernetes, HPA, Karpenter or Cluster Autoscaler, EC2 | [View Project](https://github.com/YOUR-USERNAME/YOUR-EKS-COST-OPTIMIZATION-REPO) |

## Key AWS Cost Optimization Practices

- **Right-sizing:** Match compute and database resources to actual workload requirements.
- **Schedule non-production resources:** Stop or scale down development resources when they are not needed.
- **Storage lifecycle management:** Move infrequently accessed data to appropriate storage classes and remove data according to retention requirements.
- **Autoscaling:** Adjust capacity to match demand instead of maintaining unnecessary resources.
- **Cost monitoring:** Use AWS Budgets, Cost Explorer, and alerts to track spending and identify unexpected changes.
- **Resource cleanup:** Find and remove unused resources such as unattached EBS volumes, obsolete snapshots, and unused load balancers after verifying they are no longer needed.
- **Security and governance:** Use least-privilege IAM, resource tags, and safeguards to ensure cost controls do not disrupt production workloads.

## Learning Objectives

Through these projects, I am practicing:

- AWS infrastructure and cost management
- Automation with Python and AWS Lambda
- IAM roles and least-privilege permissions
- Event-driven scheduling and monitoring
- Storage lifecycle and retention management
- Kubernetes resource management and autoscaling
- Cost measurement and documentation

## Important Cost Notes

Cost savings depend on AWS Region, resource type, usage, pricing model, and workload. Stopping an EC2 instance generally stops its instance compute charges, but attached storage and other resources may continue to incur charges. AWS Budgets provides alerts and does not automatically impose a hard spending cap.

Always review current AWS pricing and set a budget before deploying resources. Delete or stop test resources when finished, and do not include AWS access keys, credentials, account IDs, or other secrets in public repositories.

*This repository serves as the main index for my AWS Cost Optimization learning projects. Individual project implementation steps, architecture, screenshots, and results will be documented in their respective repositories.*
