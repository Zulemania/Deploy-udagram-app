# CD12352 - Infrastructure as Code Project Solution
# Udagram - High Availability Web App Deployment

## Project Overview

This Project deploys a highly available web application infrastructure on AWS using CloudFormation. The infrastructure is divided into two independent stacks:

## Infrastructure Creation Instructions

### Create the Network Stack

Deploy the network infrastructure first:

```bash
./create.sh Udagram-NetworkStack network.yml network-parameters.json
```

This creates:
- VPC 
- Public and private subnets in two availability zones
- Internet Gateway attached to VPC
- NAT Gateway with Elastic IP in public subnet
- Route tables
- Routesß



### Create the Application Stack


```bash
./create.sh udagraminfra udagram.yml udagram-parameters.json
```

This creates:
- S3 bucket with public-read access for static content
- IAM Role with S3 permissions and Instance Profile
- Security Groups for Load Balancer and Web Servers
- Launch Template with Nginx installation script
- Auto Scaling Group
- Application Load Balancer 


### Step 3: Access the Application

```bash
aws cloudformation describe-stacks --stack-name udagraminfra --query "Stacks[0].Outputs[?OutputKey=='LoadBalancerURL'].OutputValue" --output text
```

Open the URL in your browser.


### Delete the Application Stack

```bash
./delete.sh udagraminfra
```

### Step 2: Delete the Network Stack

```bash
./delete.sh Udagram-NetworkStack
```


### Update Network Stack

```bash
./update.sh Udagram-NetworkStack network.yml network-parameters.json
```

### Update Application Stack

```bash
./update.sh udagraminfra udagram.yml udagram-parameters.json
```

### Working Application URL

The application is successfully deployed and accessible at: http://udagramapp-loadbalancer-1028085160.us-east-1.elb.amazonaws.com/

The page displays: "It works! Udagram, Udacity"


## Project Files

```
network.yml: Network infrastructure template
network-parameters.json: Network stack parameters
udagram.yml: Application infrastructure template
udagram-parameters.json: Application stack parameters
create.sh: Script to create stacks
delete.sh: Script to delete stacks
update.sh: Script to update stacks
```

