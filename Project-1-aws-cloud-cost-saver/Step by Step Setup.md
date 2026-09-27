## Step 1 — Create an AWS Budget alert

1. Sign in to AWS Console and open **Billing and Cost Management**.
2. Open **Budgets** and select **Create budget**.
3. Choose a monthly cost budget.
4. Set a budget amount you can afford (for example, $10; this is only an example).
5. Configure notifications at 50%, 80%, and 100%.
6. Add an email notification / SNS subscription and confirm the email.
7. Verify the budget appears in the Budgets page.

## Step 2 — Create a private S3 bucket

1. Open **S3 → Create bucket**.
2. Choose a globally unique name, e.g. `yourname-cloud-cost-saver-reports-2026`.
3. Select a region, such as `ap-south-1`.
4. Keep **Block all public access** enabled.
5. Keep default encryption enabled and create the bucket.
6. Create two prefixes (folders): `reports/` and `demo/`.

Use your real bucket name in the Lambda environment variable and IAM policy. Do not make the bucket public.

## Step 3 — Configure S3 Lifecycle optimization

1. Open your S3 bucket → **Management** → **Create lifecycle rule**.
2. Name: `Transition-old-demo-objects`.
3. Filter Type under prefix `demo/`.
4. Lifecycle rule actions Select:
	1. Transition current versions of objects between storage classes 
		1. Standard-IA after 30 days.
	2. Delete expired object delete markers or incomplete multipart uploads
		1. Delete incomplete multipart uploads: 7days
5. Create the rule and verify it is Enabled.

## Step 4 — Create the Lambda IAM role

1. Open **IAM → Roles → Create role**.
2. Trusted entity: AWS service; use case: Lambda.
3. Attach `AWSLambdaBasicExecutionRole` for CloudWatch logging.
4. Name the role `CloudCostSaverLambdaRole`.
5. Add an inline policy using `lambda-policy.json` below.

Replace the example bucket name with your actual bucket name:

```yaml
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadCostExplorer",
      "Effect": "Allow",
      "Action": [
        "ce:GetCostAndUsage"
      ],
      "Resource": "*"
    },
    {
      "Sid": "WriteCostReports",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/reports/*"
    }
  ]
}
```

Save the inline policy as `CloudCostSaverLambdaPolicy`.

## Step 5 — Create the Lambda function

1. Open **AWS Lambda → Create function → Author from scratch**.
2. Function name: `cloud-cost-report`.
3. Choose a supported Python runtime.
4. Under permissions, select existing role `CloudCostSaverLambdaRole`.
5. Create the function.
6. Open **Configuration → Environment variables → Edit**.
7. Add `REPORT_BUCKET` with your actual S3 bucket name.

## Step 6 — Add the Python code

Save the following code:

```python
import json
import os
from datetime import date, timedelta

import boto3

# Cost Explorer API is accessed through us-east-1.
ce = boto3.client("ce", region_name="us-east-1")

# Uses the Lambda function's configured AWS region.
s3 = boto3.client("s3")

REPORT_BUCKET = os.environ["REPORT_BUCKET"]


def lambda_handler(event, context):
    # Cost Explorer's End date is exclusive.
    end_date = date.today()
    start_date = end_date - timedelta(days=30)

    response = ce.get_cost_and_usage(
        TimePeriod={
            "Start": start_date.isoformat(),
            "End": end_date.isoformat()
        },
        Granularity="MONTHLY",
        Metrics=["UnblendedCost"],
        GroupBy=[
            {
                "Type": "DIMENSION",
                "Key": "SERVICE"
            }
        ]
    )

    rows = []

    for period in response.get("ResultsByTime", []):
        for group in period.get("Groups", []):
            rows.append({
                "service": group["Keys"][0],
                "amount": group["Metrics"]["UnblendedCost"]["Amount"],
                "unit": group["Metrics"]["UnblendedCost"]["Unit"]
            })

    report = {
        "period_start": start_date.isoformat(),
        "period_end_exclusive": end_date.isoformat(),
        "service_costs": rows
    }

    key = f"reports/cost-report-{end_date.isoformat()}.json"

    s3.put_object(
        Bucket=REPORT_BUCKET,
        Key=key,
        Body=json.dumps(report, indent=2).encode("utf-8"),
        ContentType="application/json",
        ServerSideEncryption="AES256"
    )

    return {
        "statusCode": 200,
        "body": json.dumps({
            "message": "Cost report saved",
            "bucket": REPORT_BUCKET,
            "key": key,
            "service_count": len(rows)
        })
    }
```

### Deploy the code

1. In Lambda, open the **Code** tab.
2. The console's default file is `lambda_function.py`.
3. Click **Deploy**.
## Step 7 — Test Lambda

1. In Lambda, open the **Test** tab.
2. Create a test event named `cost-report-test`.
3. Use an empty JSON event:

```json
{}
```

4. Click **Test**.
5. Check for a successful response.
6. Open your S3 bucket → `reports/`.
7. Confirm that a `cost-report-YYYY-MM-DD.json` file exists.

If access is denied, check the Lambda role's Cost Explorer and S3 permissions. If the report is empty, verify Cost Explorer is enabled and billing data is available. Review CloudWatch Logs for errors.

## Step 8 — Schedule weekly reports

1. Open **Amazon EventBridge → Scheduler → Create schedule**.
2. Name: `weekly-cloud-cost-report`.
3. Choose a recurring schedule.
4. Example cron expression: `0 9 ? * MON *` (Monday at 09:00 UTC).
5. Select AWS Lambda Invoke as the target.
6. Choose `cloud-cost-report`.
7. Configure Scheduler's execution role to invoke the Lambda.
8. Create the schedule.
9. Verify invocations in Lambda Monitor and CloudWatch Logs.

09:00 UTC is 14:45 Nepal time (NPT). Use the timezone option if available and confirm the schedule's displayed next invocation.

The Amazon EventBridge will automatically Trigger the lambda function in Monday at `09:00` UTC, and cost report will generate automatically and store in s3 bucket inside reports folder.
we will download the cost report and analyze and see them.