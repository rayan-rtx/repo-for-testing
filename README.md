| Issue | Code Error | Description | Behaviors | PossibleSolutions | Notes |
|-------|------------|-------------|-----------|--------------------|-------|
| Insufficient Instance Capacity | `Insufficient Instance Capacity` | AWS does not have sufficient capacity to launch the requested EC2 instance type in the selected Availability Zone. | Creating an EC2 resource typically takes between 14 seconds and 2 minutes, while the entire infrastructure build-up process takes between 4 and 6 minutes. If you notice that the infrastructure build-up process is taking longer than this, the `InsufficientInstanceCapacity` issue is likely the cause. | Wait a few minutes and run the deployment workflow again. <br>• Change the EC2 instance type (for example, `t3.micro` to `t3.small`).<br>• Deploy the infrastructure in a different Availability Zone (for example, `eu-north-1a` to `eu-north-1b`). | This issue is more common when using AWS Free Tier instance types because they are frequently in high demand. |



## Troubleshooting

### Insufficient instance capacity

| Code Error | Description | Behaviors | Notes |
|------------|-------------|-----------|-------|
| `Sever.InsufficientInstanceCapacity` | AWS does not have sufficient capacity to launch the requested EC2 instance type in the selected Availability Zone. | Creating an EC2 resource typically takes between 14 seconds and 2 minutes, while the entire infrastructure build-up process takes between 4 and 6 minutes. If you notice that the infrastructure build-up process is taking longer than this, the `InsufficientInstanceCapacity` issue is likely the cause. | This issue is more common when using AWS Free Tier instance types because they are frequently in high demand. |

#### Possible solutions

- Wait a few minutes and run the deployment workflow again.
- Change the EC2 instance type (for example, `t3.micro` to `t3.small`).
- Deploy the infrastructure in a different Availability Zone (for example, `eu-north-1a` to `eu-north-1b`).

| Update Type       | Terraform Symbol | Examples                              |
|-------------------|------------------|---------------------------------------|
|                   |                  | Update [name] Tags                    |
|                   |                  | Update EC2 Instance Type              |
| `update-in-place` |  `~`             | Update RDS DB Identifier              |
|                   |                  | Update RDS Instance Type              |
|                   |                  | Update RDS Instance Allocated Storage |

| Update Type | Terraform Symbol | Examples                |
|-------------|------------------|-------------------------|
|             |                  | Update Subnet CIDR      |
| replace     |  `-/+`           | Update EC2 Instance AMI |
|             |                  | Update DB Subnet Group  |

| Update Type | Terraform Symbol | Examples                |
|-------------|------------------|-------------------------|
|             |                  | Create Public Subnet    |
| `create`    |  `+`             | Create Internet Gateway |
|             |                  | Create Route Table      |

| Update Type | Terraform Symbol | Examples                |
|-------------|------------------|-------------------------|
|             |                  | Remove Public Subnet    |
| `destroy`   |  `-`             | Remove Internet Gateway |
|             |                  | Remove Route Table      |



| Update Type  | Description | Examples |
|--------------|-------------|----------|
| Update In-Place | Existing resources are modified without replacement. Resource identifiers remain the same and downtime is usually minimal. | - Update resource tags<br>- Change EC2 instance type<br>- Change RDS instance class<br>- Increase RDS allocated storage<br> |
| Replace | Terraform destroys the existing resource and creates a new one. This may change IP addresses, DNS names, or resource IDs and may cause temporary downtime. | - Change EC2 AMI<br>- Modify Subnet CIDR<br>- Change DB Subnet Group |
| Create | New resources are added without affecting existing resources. | - Create a Subnet<br>- Create a Route Table<br>- Create an Internet Gateway |
| Destroy | Resources removed from the Terraform configuration are deleted from AWS. | - Remove a Subnet<br>- Remove a Route Table<br>- Remove an Internet Gateway |




![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CloudFormation](https://img.shields.io/badge/AWS-CloudFormation-orange?style=for-the-badge&logo=amazonaws)
![YAML](https://img.shields.io/badge/YAML-Template-red?style=for-the-badge&logo=yaml)



## Troubleshooting

The following table lists common issues that may occur while deploying, testing, or running the Excel processing workflow.

| Issue | Possible Cause | Solution |
|-------|----------------|----------|
| **Lambda function fails with `ModuleNotFoundError`** | The required `pandas` or `openpyxl` dependency is missing or the corresponding Lambda Layer is not attached. | Verify that both the **pandas** and **openpyxl** layers are attached to the `ProcessProductsExcel` function. |
| **Lambda function fails to import a dependency** | The Lambda Layer was built for an incompatible Python runtime or architecture. | Verify that the layer is compatible with the Lambda function's Python runtime and architecture. |
| **Lambda function returns `AccessDenied` when accessing S3** | The Lambda execution role does not have sufficient permissions to read from or write to the S3 bucket. | Review the Lambda execution role and grant the required S3 permissions. |
| **Test event fails with `NoSuchKey` or file not found** | The specified object key does not match an existing file in the S3 bucket. | Verify that the bucket name and object key are correct. For example, `inputs/products.xlsx` must exactly match the file path in S3. |
| **Lambda function is not triggered after uploading a file** | The S3 trigger is missing, disabled, or incorrectly configured. | Verify that the S3 trigger is attached to the correct Lambda function and that it listens for **ObjectCreated** events. |
| **Lambda function is triggered but does not process the uploaded file** | The uploaded file is outside the configured `inputs/` path or does not match the expected file format. | Upload the Excel file to the configured `inputs/` folder and verify that it is a valid `.xlsx` file. |
| **Lambda function times out** | The Excel file requires more processing time than the configured Lambda timeout. | Increase the Lambda execution timeout and consider increasing the allocated memory if processing larger files. |
| **Output file is not generated** | The function failed during validation, Excel processing, or the S3 upload operation. | Review the Lambda execution logs in **Amazon CloudWatch Logs** to identify the failure. |
| **Output file contains fewer records than expected** | Some rows failed the configured validation rules or duplicate records were removed. | Review the input data and verify the validation rules for required fields, price, stock, and category. |
| **Duplicate records are missing from the output** | Duplicate removal is part of the processing logic. | Verify that the removed rows contain identical product information and review the source data if necessary. |
| **Lambda test invocation fails** | The test event contains an incorrect bucket name, object key, or event structure. | Compare the test event with `tests/event.json` and update the values to match your S3 environment. |
| **Lambda function cannot retrieve the Excel file** | The S3 object does not exist or the object key is incorrect. | Confirm that the Excel file exists in the S3 bucket and that the object key matches its exact path. |
| **Excel file cannot be processed** | The uploaded file is invalid, corrupted, or not a supported `.xlsx` workbook. | Open the file locally to verify that it is valid, then upload a supported Excel file. |
| **CloudWatch Logs contain no execution logs** | The function has not been invoked, or the execution role cannot write logs to CloudWatch. | Verify that the Lambda function was invoked and that its execution role has the required CloudWatch Logs permissions. |
| **Lambda deployment does not publish the latest code** | The code changes were saved but not deployed. | After modifying the function code, select **Deploy** in the Lambda console and wait for the deployment to complete. |
| **S3 trigger configuration reports a permission error** | Lambda does not have the required permission to be invoked by Amazon S3. | Review the Lambda resource-based policy and S3 trigger configuration, then recreate or update the trigger if necessary. |
| **Processing fails for large Excel files** | The function may require additional memory, execution time, or temporary storage. | Increase the Lambda memory and timeout settings as needed and review the CloudWatch execution logs for resource-related errors. |
