# Terraform Configuration for Ecommerce Web App on AWS

This Terraform configuration deploys the infrastructure for a full-stack ecommerce web app on AWS EC2 instances behind a load balancer, with support from an RDS instance for a MySQL database.

## Resources

### `aws_vpc.ecommerce_vpc`

This resource creates a new Amazon Virtual Private Cloud (VPC) to isolate the web app and database resources. The `cidr_block` parameter specifies the IPv4 CIDR block range for the VPC.

### `aws_subnet.public_subnet`

This resource creates a new public subnet within the VPC for the EC2 instances. The `vpc_id` parameter specifies the ID of the VPC created by the `aws_vpc.ecommerce_vpc` resource, and the `cidr_block` parameter specifies the IPv4 CIDR block range for the subnet.

### `aws_subnet.private_subnet`

This resource creates a new private subnet within the VPC for the RDS instance. The `vpc_id` parameter specifies the ID of the VPC created by the `aws_vpc.ecommerce_vpc` resource, and the `cidr_block` parameter specifies the IPv4 CIDR block range for the subnet.

### `aws_security_group.webapp_security_group`

This resource creates a security group for the EC2 instances. The `name_prefix` parameter specifies a prefix for the security group name, and the `vpc_id` parameter specifies the ID of the VPC created by the `aws_vpc.ecommerce_vpc` resource. The `ingress` parameter specifies that incoming traffic on port 80 is allowed from any IPv4 address.

### `aws_security_group.rds_security_group`

This resource creates a security group for the RDS instance. The `name_prefix` parameter specifies a prefix for the security group name, and the `vpc_id` parameter specifies the ID of the VPC created by the `aws_vpc.ecommerce_vpc` resource. The `ingress` parameter specifies that incoming traffic on port 3306 is allowed only from the EC2 instances, and the `egress` parameter specifies that all outgoing traffic is allowed to any IPv4 address.

### `aws_db_subnet_group.webapp_db_subnet_group`

This resource creates a DB subnet group for the RDS instance. The `name` parameter specifies the name of the subnet group, and the `subnet_ids` parameter specifies the ID of the private subnet created by the `aws_subnet.private_subnet` resource.

### `aws_db_instance.webapp_db`

This resource creates an RDS instance for a MySQL database. The `identifier` parameter specifies the name of the RDS instance, and the `engine`, `engine_version`, `instance_class`, `allocated_storage`, `storage_type`, `storage_encrypted`, `multi_az`, and `publicly_accessible` parameters specify the configuration details for the RDS instance. The `db_subnet_group_name` parameter specifies the name of the DB subnet group created by the `aws_db_subnet_group.webapp_db_subnet_group` resource, and the `vpc_security_group_ids` parameter specifies the ID of the security group created by the `aws_security_group.rds_security_group` resource.

### `aws_instance.webapp_instance`

This resource creates an EC2 instance for the web app. The `count` parameter specifies the number of instances to create, and the `ami`, `instance_type`, `subnet_id`, and `vpc_security_group_ids` parameters specify the configuration details for the instances. The `user_data` parameter specifies the script that installs and configures the web app.

### `aws_lb.webapp_lb`

This resource creates an application load balancer for the web app. The `name` parameter specifies the name of the load balancer, and the `internal`, `load_balancer_type`, `subnets`, and `security_groups` parameters specify the configuration details for the load balancer.

### `aws_lb_target_group.webapp_target_group`

This resource creates a target group for the load balancer. The `name_prefix`, `port`, `protocol`, `target_type`, and `vpc_id` parameters specify the configuration details for the target group. The `health_check` parameter specifies the health check configuration for the target group, and the `stickiness` parameter specifies the stickiness configuration for the target group.

### `aws_lb_target_group_attachment.webapp_lb_attachment`

This resource attaches the EC2 instances to the target group for the load balancer. The `count`, `target_group_arn`, `target_id`, and `port` parameters specify the configuration details for the attachment. The `target_id` parameter specifies the ID of the EC2 instances created by the `aws_instance.webapp_instance` resource.

## Usage

To use this Terraform configuration, follow these steps:

1. Install Terraform on your local machine.
2. Clone this repository to your localmachine.
3. Go to the terraform file and change the user data CLI to the actual configuration for the web app
4. Run `terraform init` to initialize your Terraform project.
5. Run `terraform plan` to preview the changes that Terraform will make to your AWS resources.
6. If the plan looks good, run `terraform apply` to apply the changes to your AWS account.

Note that this configuration creates resources that will incur charges in your AWS account. Be sure to review the AWS pricing documentation to understand the costs associated with each resource. Also, be sure to clean up your AWS resources when you're done by running `terraform destroy`.