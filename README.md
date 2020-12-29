# Automating DevOps Workflows with GitLab and Terraform

## Overview

GitLab is a complete DevOps platform delivered as a single application. GitLab provides a single UI for development and operational teams within organizations to work concurrently throughout the application development to delivery life cycle. As a result of applications becoming more and more complex, organizations turn to automation, like CI/CD, in hopes to streamline processes. However, to increase reliability and consistency in automation beyond software development, more and more teams are resorting to declarative automation and their respective workflows in code and storing them in code repositories. Often and dynamically triggering automation as code to quickly iterate and deliver value into production is becoming more and more popular. This phenomenon is commonly known as GitOps, where tools such as Terraform and GitLab shine. 

Google Cloud provides scalable cloud infrastructure with managed services for various forms of compute, storage, networking, etc. With extensive open APIs, almost any and every aspect of it can be automated and managed via code the proper tools. Organizations looking to adopt cloud native solutions in an agile way need a better way to manage the inherent complexities when elastically scaling their services, breaking up monolithic applications, and securely operating at speed through cross functional teams. All of the prior has been the foundation of the DevOps transformation as we recognize today.

The fundamental challenge DevOps attempts to address is the unification of Developer and Operator workflow. When automating the application development process, Continuous Integration / Continuous Delivery (CI/CD) is crucial in iteratively deploying code reliably, in a secure fashion from source code into production. When automating Infrastructure administration and provisioning, Infrastructure as Code (IaC) is an emerging method to quickly and reliably build deployment targets for applications. IaC enables practitioners to deploy infrastructure through declarative means and maintain the desired state and lifecycle of cloud resources.

GitLab is the single application that provides all of the necessary functionality for DevOps teams out of the box for a seamless, low maintenance, just-commit-code software development, and delivery experience. GitLab enables teams to plan sprints and projects, organize code in git repositories, operationalize application code with CI/CD pipelines, secure source code through various vulnerability scanning, and much more. GitLab provides means to construct flexible automation workflows that are complementary to other tools like Terraform. Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently. The infrastructure Terraform can manage includes low-level components such as compute instances, storage, and networking, as well as high-level components such as DNS entries, SaaS features, etc. Together, GitLab and Terraform can be configured together to provide DevOps teams the capability to manage their cloud through IaC, continuously and reliably.

## Sample Application Overview

We will be using a sample application that we have called Vote-App which is representative of a N-Tier microservice architecture, containerized application that will be deployed to a Kubernetes cluster provided by GKE. Below is a high level overview of the sample application architecture:


![Arch](https://github.com/IamVigneshC/GCP-Automating-DevOps-Workflows-with-GitLab-and-Terraform/blob/main/Resources/arch.png)


__Vote UI:__ Is a python containerized microservice using flask to generate a front end UI for end users to place a ‘vote'. This service will be running on GKE.

__Results UI:__ Is a NodeJs containerized microservice used to generate a front end UI for end users to see voting results. This service will be running on GKE.

__Redis:__ This will be a managed Redis service powered by Cloud Memorystore which will serve as a cached queuing system for votes. This managed service will store aggregated votes inserted by the *Vote UI *service.

__Postgres:__ This will be a managed Postgres service powered by Cloud SQL which will serve as a central database for all votes casted and accounted for. This managed service will be queried by the Results UI to show results.

__Worker:__ This Is a .Net Core containerized microservice that queries votes in the que stored in Redis cache and enters them into the Postgres database. This service will be running on GKE.


## DevOps Workflow Overview

You will have access to two GitLab users, Developer and Operator. After initial GitLab configuration and post importing of all relevant projects, you will role play deploying microservices and managing Infrastructure via IaC automated through CI/CD from code commit to running in production. 

You will start with only a single, pre-provisioned GKE cluster as a deployment target for your microservices. In this example, we will simply be committing to the Master branch to deploy into a single staging environment. The high level workflow is as follows:

- The user will commit code to the application code repositories to scan, package and deploy the service.

- This will kick off a series of CI/CD pipelines to

  - Execute code security scanning for vulnerabilities
  - Package the code into a docker container
  - Trigger a child pipeline to provision dependent infrastructure.
  - Deploy all necessary Kubernetes resources (pods, services, secrets, etc.) to GKE.
  
- The Terraform child CI/CD pipeline stored will:

  - Use Terraform to init, plan, and apply to provision a Memstore instance and a Cloud SQL Postgres Instance on GCP.
  - The CI/CD pipeline will also update and deploy a ConfigMap and Secrets resource to Kubernetes that the microservices will reference for appropriate DB endpoints and credentials.
  - Lastly, the services will then be deployed to Kubernetes and be accessible through a web endpoints by the end user.
  
The workflow will be concerned with a single GitLab subgroup spanning multiple GitLab Projects outlined below. Each project listed below is a git repository with a .gitlab-ci.yml file already defined for an automated pipeline to execute upon code commit.

![Arch](https://github.com/IamVigneshC/GCP-Automating-DevOps-Workflows-with-GitLab-and-Terraform/blob/main/Resources/Voting.png)


## Objectives

You will:

- Configure relevant CI environment variables within GitLab projects to provide appropriate runtime information for Terraform CLI

- Add an existing Kuberenetes cluster as a deployment target in GitLab via GitLab’s Kubernetes integration

- Trigger parent-child CI/CD pipelines through new Merge Requests that call on pre-defined CI templates and evoke an Infrastructure as Code workflow via Terraform

- Learn how to organize GitLab Groups with multiple projects per microservice when developing and operating an N-Tier cloud native application

<BR/>

### Continue reading [here](https://iamvigneshc.medium.com/automating-devops-workflows-with-gitlab-and-terraform-6113400fa5c6)....

<BR/>
Additional Reference:
https://www.terraform.io/docs/index.html <BR/>
https://docs.gitlab.com/ee/ci/
