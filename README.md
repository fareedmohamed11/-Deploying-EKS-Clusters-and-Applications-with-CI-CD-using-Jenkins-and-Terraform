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

![Apply](https://github.com/fareedmohamed11/-Deploying-EKS-Clusters-and-Applications-with-CI-CD-using-Jenkins-and-Terraform/blob/f61e2eeab933a938a363b933981c864fdca7840c/68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f76322f726573697a653a6669743a3730302f312a3459646d76776c434b476148634446505365716d41772e706e67.png)
### Ah, I see you've successfully launched your Jenkins EC2 instance! That's excellent.

### The message "Give it some time before you go to the AWS EC2 console and check the instance status. Even though the instance is running, it may still be installing the tools" is a very important point. Cloud instances often take a few minutes after they show a "running" status to fully initialize and execute the user data script.

### The screenshot of the AWS EC2 console confirms that your instance named "Jenkins-Server" (instance ID `i-01792ce1d899168`) is indeed in the **running** state. You can see the public IPv4 address (`3.207.12.44`) assigned to it.

### The text below the image, "Jenkins Build Server Created," indicates that Terraform has successfully provisioned the EC2 instance.

### Now, as you mentioned, the next crucial step is to verify if all the tools specified in your `install_build_tools.sh` script have been installed correctly.

### To do this, you'll need to connect to the EC2 instance using SSH. As the instructions say, you'll need to select the EC2 instance in the AWS console and then get the SSH command. This command will typically include the public IP address of the instance, the username (usually `ec2-user` for Amazon Linux 2), and specify the private key file associated with the `jenkins_server_keypair` you mentioned earlier.

### Once you're connected via SSH, you can run commands to check the versions of the installed tools, such as:

* ### `#java -version`
* ### `#git --version`
* ### `#docker --version`
* ### `#aws --version`
* ### `#terraform --version`
* ### `#kubectl version --client`
* ### `#trivy --version`
* ### `#helm version`

### You can also check the status of the Jenkins and Docker services using `sudo systemctl status jenkins` and `sudo systemctl status docker`.

### Let me know if you encounter any issues when trying to connect to the instance or verifying the installed tools!
## SSH commands to log in to EC2 instance
Next, go to your terminal, and paste the `ssh` commands.  
But before this, make sure you have the key pair file downloaded in your workstation.

---

### Step-by-step

bash
### Check your current directory
$ pwd
/Users/fareed

### Move to the Downloads folder (or where your .pem file is)
$ cd Downloads

### List the key pair file to make sure it’s there
### $ ls -ltr jenkins_server_keypair.pem
-r--------@ 1 fareed  staff  1674 Jan 16 15:00 jenkins_server_keypair.pem
---

# Use the key pair to SSH into the EC2 instance
$ ssh -i "jenkins_server_keypair.pem" ec2-user@ec2-52-207-152-48.compute-1.amazonaws.com

###  First time you connect, you may see a warning like this:
### The authenticity of host 'ec2-52-207-152-48.compute-1.amazonaws.com (52.207.152.48)' can't be established.
### Are you sure you want to continue connecting (yes/no/[fingerprint])?

### Type "yes" and press Enter

### You should now be connected to your EC2 instance.

# Sample Output After Successful Login
       __|  __|_  )
       _|  (     /   Amazon Linux 2
      ___|\___|___|

  AL2 End of Life is 2025-06-30.

  A newer version of Amazon Linux is available.

# Stage 2: Create Terraform configuration files for creating the EKS Cluster
**Task 1: Create Terraform configuration files
Moving on, let's start writing terraform configurations for the EKS cluster in a private subnet.**

We'll use the same bucket but a different key/folder for the terraform remote state file.
--
**terraform {
  backend "s3" {
    bucket = "terraform-eks-cicd-7001"
    key    = "eks/terraform.tfstate"
    region = "us-east-1"
  }
}**
--
# We'll be using publicly available modules for creating different services instead of resources
# https://registry.terraform.io/browse/modules?provider=aws

# Creating a VPC
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"

  name = var.vpc_name
  cidr = var.vpc_cidr

  azs            = data.aws_availability_zones.azs.names
  public_subnets = var.public_subnets
  private_subnets = var.private_subnets

  enable_dns_hostnames = true
  enable_nat_gateway = true
  single_nat_gateway = true

  tags = {
    "kubernetes.io/cluster/my-eks-cluster" = "shared"
    Terraform   = "true"
    Environment = "dev"
  }

  public_subnet_tags = {
    "kubernetes.io/cluster/my-eks-cluster" = "shared"
    "kubernetes.io/role/elb" = 1
  }

  private_subnet_tags = {
    "kubernetes.io/cluster/my-eks-cluster" = "shared"
    "kubernetes.io/role/internal-elb" = 1
  }
}
---
# eks.tf
# Ref - https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest

module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "~> 20.0"

  cluster_name    = "my-eks-cluster"
  cluster_version = "1.29"

  cluster_endpoint_public_access  = true

  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets

  eks_managed_node_groups = {
    nodes = {
      min_size     = 1
      max_size     = 3
      desired_size = 2

      instance_types = ["t2.small"]
      capacity_type  = "SPOT"
    }
  }

  tags = {
    Environment = "dev"
    Terraform   = "true"
  }
}
--
# 📝 Note:
### We are using a private subnet for our EKS cluster as we don't want it to be publicly accessed.
**aws_region      = "us-east-1"
aws_account_id  = "12345678"
vpc_name        = "eks-vpc"
vpc_cidr        = "192.168.0.0/16"
public_subnets  = ["192.168.1.0/24", "192.168.2.0/24", "192.168.3.0/24"]
private_subnets = ["192.168.4.0/24", "192.168.5.0/24", "192.168.6.0/24"]
instance_type   = "t2.small"**

--
![jenkins](https://github.com/fareedmohamed11/-Deploying-EKS-Clusters-and-Applications-with-CI-CD-using-Jenkins-and-Terraform/blob/694b4354c8adf8575a7a644bb2cb79b58c7159ca/68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f76322f726573697a653a6669743a3730302f312a6c5a4d4a34617144766a6f555565334650767a434f512e706e67.png)
 
 ![jenkins](https://github.com/fareedmohamed11/-Deploying-EKS-Clusters-and-Applications-with-CI-CD-using-Jenkins-and-Terraform/blob/eeed69e2af83630f581e2dd90e5f08ea8c5b735f/68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f76322f726573697a653a6669743a3730302f312a68375f4247735f2d56344e6c3658452d37374b6b4f772e706e67.png)

---
## 🏁 # Conclusion

We have successfully implemented a **robust and automated infrastructure provisioning and deployment pipeline** using:

* **Terraform**
* **EKS**
* **Jenkins**

We designed and deployed:

* A **scalable and efficient CI/CD pipeline**
* A **simple Nginx application** inside the EKS cluster

> ✅ **However, this is not the end — it's just the beginning** of building more complex production-grade CI/CD systems.

