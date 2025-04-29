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
