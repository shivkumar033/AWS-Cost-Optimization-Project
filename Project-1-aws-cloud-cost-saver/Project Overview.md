## 1. Project Overview

AWS Cloud Cost Saver is a serverless cloud cost monitoring and reporting project that automatically collects AWS service-level cost data, generates JSON reports, and stores them securely in Amazon S3.

The project uses AWS Lambda and Amazon EventBridge Scheduler to automate weekly cost reporting, while AWS Budgets helps monitor spending and send budget alerts.

The main objective is to improve cloud cost visibility, identify services contributing to AWS expenditure, and support cost optimization decisions.

## 2. AWS Services Used

| AWS Service                  | Purpose                                                 |
| ---------------------------- | ------------------------------------------------------- |
| AWS Cost Explorer            | Retrieves AWS service-level cost data.                  |
| AWS Lambda                   | Runs Python code to generate cost reports.              |
| Amazon S3                    | Stores generated JSON reports privately.                |
| Amazon EventBridge Scheduler | Automatically triggers Lambda every Monday.             |
| AWS Budgets                  | Monitors spending and sends budget alerts.              |
| AWS IAM                      | Manages permissions and secure access between services. |
| Amazon CloudWatch            | Stores Lambda logs and helps troubleshoot errors.       |

## 3. Project Workflow

```bash
                AWS CLOUD COST SAVER
                         |
          +--------------+--------------+
          |                             |
    AWS Budgets                  EventBridge Scheduler
          |                             |
   Budget Alerts                  Every Monday
   (50%, 80%, 100%)                     |
                                        v
                                AWS Lambda (Python)
                                        |
                                        v
                               AWS Cost Explorer
                                        |
                              Fetch Service Costs
                                        |
                                        v
                                  Amazon S3
                                        |
                                  reports/
                                        |
                                        v
                              Download JSON Report
                                        |
                                        v
                               Cost Analysis
                                        |
                                        v
                             Identify Cost Savings


  IAM       --> Secure permissions between AWS services
  CloudWatch --> Lambda logs and error monitoring
```

## 4. Key Features

- Automated weekly AWS cost reporting.
    
- Service-wise cost breakdown in JSON format.
    
- Private S3 storage with server-side encryption.
    
- Budget threshold alerts at 50%, 80%, and 100%.
    
- S3 lifecycle rules for storage cost management.
    
- Scheduled execution without manual intervention.

## 5. Project Outcome

The project demonstrates practical knowledge of serverless architecture, AWS cost monitoring, IAM least-privilege permissions, automated scheduling, and cloud storage lifecycle management.