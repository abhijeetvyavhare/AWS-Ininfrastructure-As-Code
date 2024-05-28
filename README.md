# AWS-Ininfrastructure-As-Code

Welcome to the **AWS CloudFormation Template** repository! This repository contains a CloudFormation template for creating an Application Load Balancer and an Auto Scaling Group within a VPC configuration.

## Table of Contents
- [Description](#description)
- [Parameters](#parameters)
- [Resources](#resources)
- [Outputs](#outputs)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Description
This CloudFormation template provisions the following resources:
- VPC with public and private subnets
- Internet Gateway and associated route tables
- EC2 Launch Template with Apache and MySQL setup
- Application Load Balancer (ALB)
- Auto Scaling Group (ASG) with target group and listener

## Parameters
The template requires the following parameters:

- **KeyName**: Name of an existing EC2 key pair to enable SSH access to the instance.
- **VpcCIDR**: CIDR block for the VPC. Default is "10.0.0.0/16".

## Resources
The template creates the following resources:

### Networking Resources
- **MyVPC**: A Virtual Private Cloud with the specified CIDR block.
- **PublicSubnet**: A public subnet within the VPC.
- **PrivateSubnet**: A private subnet within the VPC.
- **InternetGateway**: An Internet Gateway attached to the VPC.
- **PublicRouteTable**: A route table for the public subnet.
- **PublicRoute**: A route in the route table directing traffic to the Internet Gateway.
- **SubnetRouteTableAssociation**: Associates the public subnet with the public route table.

### Security Resources
- **InstanceSecurityGroup**: A security group allowing HTTP, SSH, and MySQL access.

### Compute Resources
- **LaunchTemplate**: An EC2 launch template with the user data script for installing Apache, MySQL, and deploying a web application.
- **AutoScalingGroup**: An Auto Scaling group configured with the launch template and associated with the target group.

### Load Balancing Resources
- **TargetGroup**: An ALB target group to route traffic to EC2 instances.
- **LoadBalancer**: An internet-facing Application Load Balancer.
- **Listener**: An ALB listener configured to forward HTTP traffic to the target group.

## Outputs
The template provides the following output:
- **LoadBalancerDNSName**: The DNS name of the load balancer.

## Getting Started
To deploy this CloudFormation stack, follow these steps:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/your-repo-name.git
   cd your-repo-name
   ```

2. **Validate the CloudFormation template:**
   ```bash
   aws cloudformation validate-template --template-body file://template.yml
   ```

3. **Deploy the CloudFormation stack:**
   ```bash
   aws cloudformation create-stack --stack-name my-stack --template-body file://template.yml --parameters ParameterKey=KeyName,ParameterValue=your-key-name ParameterKey=VpcCIDR,ParameterValue=10.0.0.0/16 --capabilities CAPABILITY_NAMED_IAM
   ```

## Usage
Once the stack is created, you can access the web application using the load balancer's DNS name provided in the stack outputs.

### Example: Accessing the Application
1. Retrieve the LoadBalancerDNSName from the CloudFormation stack outputs.
   ```bash
   aws cloudformation describe-stacks --stack-name my-stack --query "Stacks[0].Outputs[?OutputKey=='LoadBalancerDNSName'].OutputValue" --output text
   ```
2. Open a web browser and navigate to `http://<LoadBalancerDNSName>`.

Thank you for using the **AWS CloudFormation Template** repository. If you have any questions or issues, please feel free to open an issue or contact me. Happy coding!
