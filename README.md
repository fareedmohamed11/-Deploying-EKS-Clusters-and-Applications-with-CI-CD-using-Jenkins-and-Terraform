# -Deploying-EKS-Clusters-and-Applications-with-CI-CD-using-Jenkins-and-Terraform

![Deploying-EKS-Clusters-and-Applications-with-CI-CD-using-Jenkins-and-Terraform](https://github.com/fareedmohamed11/-Deploying-EKS-Clusters-and-Applications-with-CI-CD-using-Jenkins-and-Terraform/blob/20dd743064a3f065309758ba9e695e85a072f2d5/image.png)

# Streamlining EKS Deployment and CI/CD: A Step-by-Step Guide to Automating Application Delivery with Jenkins and Terraform

Welcome to this step-by-step guide on deploying an EKS cluster and application with complete CI/CD!

In this guide, we’ll streamline your application delivery process by automating your infrastructure deployment. You’ll learn how to set up a fully functional EKS cluster and a simple containerized application and run it with a CI/CD pipeline that automates the entire process from code to production.

Let’s get started and explore the world of EKS, CI/CD, and automation!

## What We’ll Build
We are going to build and deploy a lot of things. Here is the outline for our project:

1. Setting up Jenkins Server with Terraform
2. Installing necessary tools: Java, Jenkins, AWS CLI, Terraform CLI, Docker, Sonar, Helm, Trivy, Kubectl.
3. Configuring Jenkins server.
## II. Creating EKS Cluster with Terraform
- Writing Terraform configuration files for EKS cluster creation in a private subnet.
- Deploying EKS cluster using Terraform.

## III. Deploying NGINX Application with Kubernetes
- Writing Kubernetes manifest files (YAML) for the NGINX application.
- Deploying NGINX application to EKS cluster.

## IV. Automating Deployment with Jenkins CI/CD
- Creating Jenkins pipeline for automating EKS cluster and NGINX application deployment.
- Integrating Terraform and Kubernetes with the Jenkins pipeline.

## What We’ll Need
To embark on our CI/CD adventure, we’ll need to trustfully provision the following tools:
- **Terraform** — To create the infrastructure for the EC2 instance which will be used as a Jenkins server and EKS cluster in AWS.
- **Shell Script** — To create a pipeline in the EC2 instance.
- **Jenkins File** — To create a pipeline in the Jenkins Server.
## Source Code
You can download the complete source code inside this repository.

## Prerequisites
Before creating and working with the project, let’s set up some tools:

1. Make sure you have an IDE to develop your project. I am using Visual Studio Code for the same. You can install it from the following link based on the operating system: [Visual Studio Code](https://code.visualstudio.com/download).

2. Install the CLI tools — [AWS-CLI](https://aws.amazon.com/cli/) and [Terraform-CLI](https://www.terraform.io/downloads.html).

3. Make sure you have an AWS Free Tier account. And then create a user in IAM Console and finally create an Access Key ID and Secret Access Key as shown below (copy this from the credentials file downloaded):

   ```bash
   export AWS_ACCESS_KEY_ID=<Copy this from the credentials file downloaded>
   export AWS_SECRET_ACCESS_KEY=<Copy this from the credentials file downloaded>
## Stage 1: Configure and Build Jenkins Server
The first thing we have to do is to create a new key pair for login into the EC2 instance and create an S3 bucket for storing terraform state files. This is the only manual step we are doing.

So, in the AWS management console, go to `EC2` and select `key pairs` in the listed overview of your resources, and then select "Create key pair" at the top right corner. You need to download these key pairs so that you can use them later for logging into the EC2 instance.

## Create Key Pairs for the EC2 Instance
Next, let's create an S3 bucket to store the terraform remote states. You can also create a S3 bucket via Terraform, but in that case, you need to apply this configuration first as the S3 bucket must already exist before using it as a remote backend in Terraform. 

Hence, go to `S3` and create a bucket named `terraform-eks-cid-<random_number>` (use some random number at the end to make it unique).
# Terraform Configuration for Jenkins Server on AWS

# This repository contains the Terraform configuration to provision an EC2 instance that will serve as a Jenkins server.

## #Prerequisites

# Before running `terraform apply`, ensure the following:

* #AWSAccount: You have an active AWS account configured with the necessary permissions.
* #AWSCLI: The AWS Command Line Interface is installed and configured with your AWS credentials.
* #Terraform: Terraform version 0.13 or later is installed on your local machine.
* #KeyPair: An EC2 key pair named `jenkins_server_keypair` exists in the `us-east-1` region. You can create one using the AWS Management Console or AWS CLI.

## #Setup

1.  #CreateS3BucketForRemoteState:
    ```bash
    aws s3api create-bucket --bucket terraform-eks-cicd-7001 --region us-east-1
    ```
    *(#RememberToChooseAGloballyUniqueBucketNameIfThisOneIsAlreadyTaken)*

2.  #CloneTheRepository:
    ```bash
    git clone <your_repository_url>
    cd <repository_directory>/jenkins_server/tf-aws-ec2
    ```

## #TerraformConfigurationFiles

* `#backend.tf`: Configures the Terraform S3 backend.
* `#data.tf`: Retrieves AWS Availability Zones and the latest Amazon Linux 2 AMI (though a specific AMI is used in `main.tf`).
* `#main.tf`: Defines the AWS resources, including the VPC, security group, and EC2 instance.
* `#variables/dev.tfvars`: Contains the variable values for the `dev` environment (as indicated by your `terraform apply` command).
* `#scripts/install_build_tools.sh`: A shell script executed on the EC2 instance at launch to install Jenkins, Git, Docker, AWS CLI, Terraform, Kubectl, Trivy, and Helm.

## #RunningTerraform

1.  #InitializeTerraform:
    ```bash
    terraform init
    ```

2.  #ApplyTheConfiguration:
    ```bash
    terraform apply -var-file=variables/dev.tfvars -auto-approve
    ```

## #ImportantNotes

* #EnsureTheKeyPairNameInMainTf (`jenkins_server_keypair`) matches an existing key pair in your AWS account in the `us-east-1` region.
* #TheS3BucketNameInBackendTf (`terraform-eks-cicd-7001`) must be unique.
* #TheUserDataInTheEC2ModuleInMainTf references the `install_build_tools.sh` script, which will be executed upon instance creation.
* #DoubleCheckYourCurrentWorkingDirectory when running Terraform commands. You should be in the `tf-aws-ec2` directory.
* #AWSAccessKeyIDAndSecretAccessKey should be configured as environment variables or via AWS CLI configuration.
