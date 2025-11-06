
# **Automating Infrastructure Deployment – Challenge Lab (AWS Cloud Architecting)**

This repository documents my work for the **AWS Academy Cloud Architecting – Challenge Lab: Automating Infrastructure Deployment**.
The lab demonstrates how to transition from **manual AWS configuration** to a fully automated **Infrastructure as Code (IaC)** and **CI/CD** workflow using AWS-native tools.

---

## ✅ **Overview**

The café team wants to scale their web presence across multiple AWS Regions and maintain consistent **development** and **production** environments.
To achieve this, the deployment process is automated using:

* **AWS CloudFormation**
* **AWS CodeCommit**
* **AWS CodePipeline**
* **AWS Command Line Interface (CLI)**
* **Amazon EC2 / S3 / VPC**

This lab walks through building:

1. A **static S3 website**
2. A **VPC (network layer) using IaC**
3. A **dynamic EC2-based café application**
4. A full **CI/CD workflow** for automated stack deployment
5. **Multi-Region duplication** of both network and app stacks

---

## ✅ **Key Objectives**

### **1. Build an S3 Static Website using CloudFormation**

* Write a CloudFormation template from scratch
* Create an S3 bucket via AWS CLI
* Enable static website hosting
* Upload web assets programmatically
* Update the CloudFormation stack
* Add Outputs (website URL)

### **2. Store Templates in Git (CodeCommit)**

* Clone a CodeCommit repository
* Push CloudFormation templates
* Track version history

### **3. Deploy VPC and EC2 App Using CodePipeline**

Two AWS pipelines were used:

#### **• CafeNetworkPipeline**

Creates:

* VPC
* Public Subnet
* Routing resources
* Security group
* Exported Outputs (SubnetId, VPCId)

#### **• CafeAppPipeline**

Creates:

* EC2 instance with Apache, MariaDB & PHP
* Security group
* Automatically installs the dynamic café application
* Outputs the public IP of the web server

### **4. Duplicate the Entire Deployment to a Second Region**

* Recreate the network stack via CLI (us-west-2)
* Create Region-specific EC2 key pairs
* Deploy the application stack using the same template
* Verify that the application runs in multiple Regions

---

## ✅ **Architecture**

### **Network Layer**

* 1× VPC
* 1× Public Subnet
* Internet Gateway
* Route Table + Route
* Security Group

### **Application Layer**

* Amazon Linux 2 EC2 instance
* LAMP stack installed via UserData
* PHP app deployment via S3 script
* Public IP exposed for web access

---

## ✅ **CI/CD Workflow**

1. Developer updates a CloudFormation template
2. Runs `git add`, `git commit`, `git push`
3. **CodeCommit** receives the commit
4. **CodePipeline** automatically triggers
5. **CloudFormation** deploys/updates the stack
6. EC2 launches + installs app automatically

This mirrors a real-world DevOps pipeline.

---

## ✅ **Commands Used**

### **Create Stack**

```bash
aws cloudformation create-stack --stack-name CreateBucket --template-body file://S3.yaml
```

### **Update Stack**

```bash
aws cloudformation update-stack --stack-name CreateBucket --template-body file://S3.yaml
```

### **Validate Template**

```bash
aws cloudformation validate-template --template-body file://S3.yaml
```

### **Git Workflow**

```bash
git add .
git commit -m "update template"
git push
```

---

## ✅ **Multi-Region Deployment**

Deploy network stack in Oregon:

```bash
aws cloudformation create-stack \
  --stack-name update-cafe-network \
  --template-body file:///home/ec2-user/environment/CFTemplatesRepo/templates/cafe-network.yaml \
  --region us-west-2
```

Upload app template to S3, then create stack via console using the S3 URL.

---

## ✅ **Skills Demonstrated**

* Infrastructure as Code (IaC)
* CloudFormation template authoring
* VPC architecture
* CI/CD with CodeCommit & CodePipeline
* Linux UserData automation
* EC2 management
* S3 static hosting
* Cross-stack references (Outputs and ImportValue)
* Multi-Region architecture
* Git workflows
* AWS CLI automation

---

## ✅ **Conclusion**

This challenge lab shows how CloudFormation and CI/CD fully transform deployment workflows.
Using automation enables:

* Consistent deployments
* Faster environment provisioning
* Easier multi-Region scaling
* Better DevOps practices
* Stronger disaster recovery readiness

The final outcome is a fully automated, multi-Region café application deployed entirely through Infrastructure as Code.

---
