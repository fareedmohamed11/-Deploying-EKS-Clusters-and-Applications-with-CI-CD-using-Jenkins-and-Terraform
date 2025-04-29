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
