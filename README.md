
![Alt text](/Host_a_Dynamic_Website_on_AWS_with_ECS.png)

![Alt text](/VPC_drawing.png)


# Hosting a Dynamic Website on AWS Using Docker, ECS, and ECR

This project demonstrates how to host a dynamic website on AWS using Docker containers, ECS (Elastic Container Service), and ECR (Elastic Container Registry). The reference diagram and configuration files are available in a private GitHub repository.

## Prerequisites

1. **Install Git, Docker, and Visual Studio Code**:
   - Download and install Git from [git-scm.com](https://git-scm.com).
   - Download and install Docker from [docker.com](https://www.docker.com).
   - Download and install Visual Studio Code from [code.visualstudio.com](https://code.visualstudio.com).

2. **GitHub Account**:
   - Register for a GitHub account at [github.com](https://github.com).
   - Create two private GitHub repositories: one for the Dockerfile and another for the application code.

3. **AWS CLI**:
   - Install AWS CLI from [aws.amazon.com/cli](https://aws.amazon.com/cli).
   - Create an IAM User and configure authentication for AWS CLI.

## Steps to Deploy

### Step 1: Docker Setup

1. **Creating Dockerfile**:
   - Create a Dockerfile to build a container image for the dynamic website:

     ```Dockerfile
     # Use an official PHP image as a parent image
     FROM php:7.4-apache

     # Copy application source code to the container
     COPY . /var/www/html/

     # Install required PHP extensions
     RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
     ```

2. **Build the Docker Image**:

    ```sh
    docker build -t your-dockerhub-username/your-image-name .
    ```

3. **Run the Container Locally**:

    ```sh
    docker run -p 80:80 your-dockerhub-username/your-image-name
    ```

### Step 2: GitHub and Application Code

1. **Repository Setup**:
   - Clone the two private repositories to your local machine:

     ```sh
     git clone git@github.com:your-username/dockerfile-repo.git
     git clone git@github.com:your-username/application-code-repo.git
     ```

2. **Sync Changes**:
   - Use Visual Studio Code to write and edit scripts, then sync changes with GitHub.

### Step 3: Database Migration

1. **Flyway Setup**:
   - Use Flyway to migrate SQL data into the RDS database.

    ```sh
    flyway -url=jdbc:mysql://your-rds-endpoint:3306/your-db-name -user=your-username -password=your-password migrate
    ```

### Step 4: AWS Setup

1. **Create Amazon ECR Repository**:
   - Create an ECR repository to store your Docker image.

2. **Push Docker Image to ECR**:

    ```sh
    aws ecr get-login-password --region your-region | docker login --username AWS --password-stdin your-aws-account-id.dkr.ecr.your-region.amazonaws.com
    docker tag your-dockerhub-username/your-image-name:latest your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-ecr-repository-name:latest
    docker push your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-ecr-repository-name:latest
    ```

3. **Create VPC**:
   - Create a 3-tier VPC with public and private subnets in 2 availability zones.

### Step 5: Security and Networking

1. **Create Security Groups**:
   - Define security groups to control inbound and outbound traffic to/from resources.

2. **Create Internet Gateway**:
   - Allow communication between VPC resources and the Internet.

3. **Create NAT Gateways**:
   - Allow resources in the private subnets to access the internet.

### Step 6: Load Balancing and Database

1. **Create Application Load Balancer**:
   - Set up an Application Load Balancer with a target group to distribute web traffic to the ECS Fargate tasks.

2. **Create Amazon MySQL RDS**:
   - Create an Amazon RDS MySQL instance for storing the website database.

### Step 7: Bastion Host

1. **Set Up Bastion Host**:
   - Use a Bastion Host to set up an SSH tunnel and securely migrate the SQL data into RDS.

### Step 8: Storing Environment Variables

1. **Create S3 Bucket**:
   - Store files containing environment variables for the container in an S3 bucket.

### Step 9: IAM Role and ECS Setup

1. **Create IAM Role**:
   - Grant permissions to ECS to execute tasks, including accessing the S3 bucket with the environment variables file.

2. **Create ECS Task and Service**:
   - Create an ECS task and service to containerize the web application on AWS Cloud and scale according to the policy.

3. **Using ECS Fargate**:
   - Run the containerized application using ECS Fargate.

### Step 10: DNS and SSL

1. **Using Route 53**:
   - Register a domain name and create a record set using Route 53.

2. **Using Certificate Manager**:
   - Enable SSL communication using AWS Certificate Manager.

## Repository

Find all configuration files, diagrams, and scripts in the public GitHub repository [https://github.com/galkini/host-dynamic-website-aws-ecs].

## Conclusion

By following the steps outlined above, this project successfully hosted a dynamic website on AWS using Docker, ECS, and ECR. This setup leverages various AWS services to ensure high availability, security, and efficient resource management.
