## IAM

### Service Role

AWS Identity and Access Management (IAM) `service roles` are special kinds of IAM roles that an `AWS service` assumes to perform actions on your behalf. Service roles provide permissions to AWS services so that they can perform tasks you specify.

Example Scenario:

- Suppose you have an Amazon EC2 instance that needs to read data from an S3 bucket. Instead of hardcoding access keys in your instance or manually granting access, you can create an IAM role with the necessary S3 read permissions and attach that role to your EC2 instance. The EC2 instance then `assumes` the role and uses its permissions to access the S3 bucket.
- You have an AWS Lambda function that processes images uploaded to an S3 bucket and stores the results in a DynamoDB table.
  - Create a service role for Lambda.
  - Attach a policy that allows the Lambda function to read from the S3 bucket and write to the
  - When the Lambda function runs, it `assumes` this role and uses the permissions to access S3 and DynamoDB.
- You have a containerized application running on Amazon ECS that needs to fetch secrets from AWS Secrets Manager and write logs to Amazon CloudWatch.
  - Create an ECS task role.
  - Attach a policy that grants permissions to read secrets from Secrets Manager and write logs to CloudWatch.
  - Assign this role to your ECS task definition so that the containers can access Secrets Manager and CloudWatch securely.

### PassRole

The `PassRole` permission in AWS Identity and Access Management (IAM) allows a user or service to pass an IAM role to an AWS service. This means that the user or service can specify which IAM role an AWS service (like EC2, Lambda, ECS, etc.) should assume to perform actions on their behalf. The PassRole permission is crucial for ensuring that only authorized users or services can delegate roles with specific permissions.

Example Scenario:

- Suppose you have a user who needs to launch EC2 instances with a specific IAM role that grants permissions to access S3 buckets.
  - Create the Role: Create an IAM role with a policy that allows access to S3 buckets.(Role Name : S3AccessRole)
  - Create a Policy with PassRole: Create an IAM policy that allows the user to pass the role to the EC2 service.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::123456789012:role/S3AccessRole"
    },
    {
      "Effect": "Allow",
      "Action": ["ec2:RunInstances", "ec2:TerminateInstances"],
      "Resource": "*"
    }
  ]
}
```

- Attach the Policy to the User: Attach the policy with the PassRole permission to the user.

The `PassRole` permission is required only when a user or service needs to delegate an IAM role to an AWS service. A user can still execute many service actions without needing PassRole permissions, but if the action involves passing an IAM role to a service, then PassRole is necessary.
