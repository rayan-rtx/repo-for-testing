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
| `replace`   |  `-/+`           | Update EC2 Instance AMI |
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
