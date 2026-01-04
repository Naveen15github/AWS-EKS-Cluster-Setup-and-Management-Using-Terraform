# AWS-EKS-Cluster-Setup-and-Management-Using-Terraform

## Project Overview

In this project, **I designed and implemented an end-to-end AWS EKS (Elastic Kubernetes Service) cluster using Terraform** to demonstrate Infrastructure as Code (IaC), Kubernetes orchestration, and AWS cloud best practices.

![ ](https://github.com/Naveen15github/AWS-EKS-Cluster-Setup-and-Management-Using-Terraform/blob/8b7cc413769251372f11b6a18bb9e90e0d37b1a0/Gemini_Generated_Image_htppaqhtppaqhtpp.png)

The goal of this project was to **provision a scalable, repeatable, and production-ready Kubernetes infrastructure** on AWS, starting from networking (VPC) to a fully functional EKS cluster with managed worker nodes.
All resources were created, verified, and managed **entirely using Terraform**, without any manual configuration in the AWS Console.

This project represents a **complete hands-on implementation**, not a theoretical or demo setup.

---

## What I Implemented

As part of this project, I implemented:

* A custom AWS VPC with proper subnet segmentation
* Public, private, and intra subnets following EKS best practices
* NAT Gateway and routing for private node access
* An AWS EKS cluster using Terraform modules
* Managed node groups with both on-demand and spot instances
* Required EKS add-ons for networking and cluster operations
* kubectl access and workload deployment
* Complete lifecycle management using Terraform (create, verify, destroy)

---

## Architecture Overview

The architecture I built includes:

* **Custom VPC**

  * Public subnets for load balancers
  * Private subnets for worker nodes
  * Intra subnets for EKS internal communication
* **AWS EKS Cluster**

  * Managed Kubernetes control plane
  * Publicly accessible API endpoint
* **EKS Managed Node Groups**

  * On-demand instances for stable workloads
  * Spot instances for cost optimization
* **EKS Add-ons**

  * CoreDNS
  * kube-proxy
  * Amazon VPC CNI

This architecture follows AWS-recommended patterns for **security, scalability, and reliability**.

---

## Prerequisites I Used

Before implementing the project, I ensured the following were set up:

### AWS Account

* Active AWS account
* IAM permissions for:

  * EKS
  * EC2
  * VPC
  * IAM
  * CloudWatch

### Terraform

I installed Terraform locally and verified it using:

```bash
terraform version
```

### AWS CLI

I installed and configured AWS CLI with my credentials:

```bash
aws configure
```

### kubectl

I installed kubectl to interact with the EKS cluster:

```bash
kubectl version --client
```

---

## Project Structure

```plaintext
.
├── provider.tf
├── vpc.tf
├── eks.tf
└── README.md
```

I intentionally split the Terraform configuration into separate files to maintain **clean structure, readability, and modularity**.

---

## provider.tf – Provider and Global Configuration

In `provider.tf`, I defined:

* AWS provider configuration
* Local variables such as:

  * AWS region
  * EKS cluster name
  * VPC CIDR block
  * Availability zones
  * Subnet CIDR ranges

Using local variables allowed me to **centralize configuration** and make the setup easily reusable for other environments.

---

## vpc.tf – VPC and Networking Setup

In `vpc.tf`, I provisioned the complete networking layer using the Terraform AWS VPC module.

### What I Created

* A custom VPC
* Public subnets
* Private subnets
* Intra subnets
* Internet Gateway
* NAT Gateway
* Route tables and associations

### Why I Designed It This Way

* Worker nodes run in **private subnets** for security
* Public subnets are reserved for **load balancers**
* NAT Gateway allows private nodes to access the internet
* Intra subnets support **EKS control plane communication**

This design aligns with **AWS EKS networking best practices**.

---

## eks.tf – EKS Cluster and Node Groups

In `eks.tf`, I created the EKS cluster using the Terraform EKS module.

### EKS Cluster Configuration

* Defined the cluster name and Kubernetes version
* Enabled public endpoint access for learning and testing
* Attached the cluster to private and intra subnets
* Integrated the cluster with the custom VPC

### EKS Add-ons

I enabled the following add-ons as part of the cluster setup:

* CoreDNS
* kube-proxy
* Amazon VPC CNI

These add-ons are critical for pod networking, service discovery, and cluster stability.

---

### Node Groups Configuration

I configured two different managed node groups:

#### On-Demand Node Group

* Used for stable and critical workloads
* Fully managed by AWS
* Provides predictable performance

#### Spot Instance Node Group

* Used for cost optimization
* Suitable for fault-tolerant workloads
* Demonstrates real-world cost-saving strategies

---

## Deployment Steps I Followed

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd terraform
```

---

### Step 2: Initialize Terraform

```bash
terraform init
```

This downloaded all required providers and modules and prepared the working directory.

---

### Step 3: Review the Execution Plan

```bash
terraform plan
```

I used this step to verify:

* What resources would be created
* Configuration correctness
* Potential issues before deployment

---

### Step 4: Apply the Configuration

```bash
terraform apply
```

After reviewing the plan, I confirmed the deployment by typing `yes`.

Terraform then:

* Created the VPC and networking components
* Provisioned the EKS control plane
* Launched managed node groups
* Configured cluster add-ons

The complete deployment took approximately **10–15 minutes**.

---

## Verification I Performed

After deployment, I verified everything manually:

### EKS Cluster

* Checked cluster status in the AWS EKS console
* Confirmed the cluster was in **ACTIVE** state

### Networking

* Verified VPC, subnets, and route tables
* Confirmed NAT Gateway configuration

### EC2 Instances

* Verified worker nodes were running under EC2 instances

---

## Interacting with the EKS Cluster

### Update kubeconfig

```bash
aws eks update-kubeconfig --region <region> --name <cluster-name>
```

This configured kubectl to communicate with my EKS cluster.

---

### Verify Cluster Connectivity

```bash
kubectl get pods -A
```

This confirmed that system pods were running correctly.

---

### Deploy a Sample Application

```bash
kubectl run nginx --image=nginx
```

---

### Verify the Deployment

```bash
kubectl get pods
```

The nginx pod moved to the **Running** state, confirming successful cluster operation.

---

## Proof of Work

This section provides **verifiable evidence** that the AWS EKS infrastructure was **fully implemented, tested, and managed by me using Terraform**, without manual resource creation.

---

### Terraform Initialization Evidence

I initialized the Terraform working directory and successfully downloaded all required providers and modules.

```bash
terraform init
```

**Result:**

* Terraform backend initialized successfully
* AWS provider and EKS/VPC modules downloaded
* Working directory ready for execution

---

### Terraform Plan Validation

Before creating any resources, I reviewed the execution plan to validate infrastructure changes.

```bash
terraform plan
```

**Verified Outputs:**

* VPC, subnets, route tables, and NAT Gateway creation
* EKS cluster provisioning
* Managed node groups (on-demand and spot)
* IAM roles and security groups

This step confirmed that the infrastructure matched the intended architecture.

---

### Terraform Apply Execution

I provisioned the complete infrastructure using Terraform.

```bash
terraform apply
```

**Outcome:**

* Custom VPC created
* EKS control plane provisioned
* Managed node groups launched
* EKS add-ons installed automatically

⏱️ Total provisioning time: approximately **10–15 minutes**

---

### AWS EKS Cluster Verification

After deployment, I verified the cluster directly in the AWS Console:

* EKS cluster status: **ACTIVE**
* Node groups status: **ACTIVE**
* EC2 worker nodes running
* Subnets and routing correctly configured

This confirmed that Terraform successfully provisioned all AWS resources.

---

### kubeconfig Configuration Proof

I configured local Kubernetes access using AWS CLI.

```bash
aws eks update-kubeconfig --region <region> --name <cluster-name>
```

**Result:**

* kubeconfig updated successfully
* kubectl authenticated with EKS cluster

---

### Kubernetes Cluster Validation

I validated cluster connectivity and system components using kubectl.

```bash
kubectl get pods -A
```

**Verified:**

* CoreDNS pods running
* kube-proxy running
* VPC CNI pods running
* No pending or failed system pods

This confirmed that the cluster was operational.

---

### Application Deployment Proof

I deployed a sample Nginx pod to validate workload scheduling.

```bash
kubectl run nginx --image=nginx
kubectl get pods
```

**Result:**

* Nginx pod successfully created
* Pod transitioned to **Running** state
* Node scheduling and networking working as expected

---

### Infrastructure Teardown Proof

After completing verification, I destroyed all AWS resources to avoid unnecessary costs.

```bash
terraform destroy --auto-approve
```

**Outcome:**

* EKS cluster deleted
* Node groups terminated
* VPC and networking resources removed
* No orphaned AWS resources remaining

---

### Screenshots 

The following screenshots can be added to further strengthen proof of work:

* Terraform `apply` success output
* EKS cluster status in AWS Console
* EC2 worker nodes running
* `kubectl get pods -A` output
* Nginx pod in running state
![EKS Cluster Active](https://github.com/Naveen15github/AWS-EKS-Cluster-Setup-and-Management-Using-Terraform/blob/8b7cc413769251372f11b6a18bb9e90e0d37b1a0/Screenshot%20(337).png)
![Kubectl Pods](https://github.com/Naveen15github/AWS-EKS-Cluster-Setup-and-Management-Using-Terraform/blob/8b7cc413769251372f11b6a18bb9e90e0d37b1a0/Screenshot%20(338).png)
![Kubectl Pods](https://github.com/Naveen15github/AWS-EKS-Cluster-Setup-and-Management-Using-Terraform/blob/8b7cc413769251372f11b6a18bb9e90e0d37b1a0/Screenshot%20(339).png)
![Kubectl Pods](https://github.com/Naveen15github/AWS-EKS-Cluster-Setup-and-Management-Using-Terraform/blob/8b7cc413769251372f11b6a18bb9e90e0d37b1a0/Screenshot%20(340).png)
![Kubectl Pods](https://github.com/Naveen15github/AWS-EKS-Cluster-Setup-and-Management-Using-Terraform/blob/8b7cc413769251372f11b6a18bb9e90e0d37b1a0/Screenshot%20(341).png)
![Kubectl Pods](https://github.com/Naveen15github/AWS-EKS-Cluster-Setup-and-Management-Using-Terraform/blob/8b7cc413769251372f11b6a18bb9e90e0d37b1a0/Screenshot%20(342).png)

---

## Destroying the Infrastructure

Once testing was complete, I cleaned up all AWS resources to avoid unnecessary costs:

```bash
terraform destroy --auto-approve
```

This removed:

* EKS cluster
* Worker nodes
* VPC and networking resources

---

## Key Learnings

From this project, I gained hands-on experience with:

* Terraform-based AWS infrastructure provisioning
* EKS cluster architecture and networking
* Managed Kubernetes node groups
* Cost optimization using spot instances
* End-to-end Kubernetes lifecycle management

---

## Future Improvements

I plan to extend this project by:

* Making the EKS API endpoint private
* Implementing IAM Roles for Service Accounts (IRSA)
* Adding ALB Ingress Controller
* Integrating monitoring with Prometheus and Grafana
* Deploying applications using CI/CD pipelines

## Author

**Naveen**
Aspiring Cloud & DevOps Engineer
AWS | Terraform | Kubernetes


