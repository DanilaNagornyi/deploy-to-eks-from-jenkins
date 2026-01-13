# Deploy to EKS from Jenkins

![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![AWS](https://img.shields.io/badge/Amazon_AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Maven](https://img.shields.io/badge/Apache%20Maven-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)

## 📋 Project Overview

This repository contains configuration files and instructions for setting up automated deployment from Jenkins to Amazon EKS (Elastic Kubernetes Service). The deployment pipeline pulls a Java Maven application from the [jenkins-java-maven](https://gitlab.com/twn-devops-bootcamp/latest/11-eks/jenkins-java-maven) repository (branch `deploy-on-k8s`) and deploys it to an EKS cluster.

## 🏗️ Architecture

- **Config Repository**: gitlab.com/twn-devops-bootcamp/latest/11-eks/deploy-to-eks-from-jenkins
- **Application Repository**: jenkins-java-maven (branch `deploy-on-k8s`)
- **CI/CD**: Jenkins
- **Container Orchestration**: Amazon EKS
- **Authentication**: AWS IAM Authenticator
- **Build Tool**: Maven
- **Language**: Java

## 🚀 Features

- Kubernetes config file template for EKS cluster access
- Jenkins server setup instructions
- Automated deployment pipeline configuration
- AWS IAM authentication setup
- ✅ Tested with Nginx deployment

## 📦 Prerequisites

- AWS Account with EKS cluster running
- Jenkins server (running in Docker recommended)
- AWS IAM credentials with EKS access
- Access to jenkins-java-maven repository

## ⚙️ Setup

### 1. Install kubectl on Jenkins Server

Install kubectl for Kubernetes cluster management:

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl
```

### 2. Install aws-iam-authenticator on Jenkins Server

Install AWS IAM Authenticator for EKS authentication:

```bash
curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.6.11/aws-iam-authenticator_0.6.11_linux_amd64
chmod +x ./aws-iam-authenticator
mv ./aws-iam-authenticator /usr/local/bin
```

### 3. Configure Kubernetes Config File

Update `config.yaml` with your EKS cluster details:
- Replace `<certificate-data>` with your cluster's CA certificate
- Replace `<endpoint-url>` with your EKS cluster endpoint
- Replace `<cluster-name>` with your EKS cluster name

### 4. Copy Config File to Jenkins Server

If Jenkins is running in Docker:

```bash
docker cp config.yaml <JENKINS_CONTAINER_ID>:/var/jenkins_home/.kube/
```

### 5. Configure Jenkins Credentials

In Jenkins, add:
- AWS credentials (Access Key ID and Secret Access Key)
- Git credentials for repository access

### 6. Create Jenkins Pipeline

Create a Jenkins pipeline job that:
1. Pulls from jenkins-java-maven repository
2. Checks out `deploy-on-k8s` branch
3. Builds and deploys to EKS

## 🔧 Deployment Process

1. **Code Push**: Developer pushes code to `deploy-on-k8s` branch
2. **Jenkins Trigger**: Jenkins detects changes and starts the pipeline
3. **Build**: Maven builds the Java application
4. **Containerize**: Docker image is created
5. **Push**: Image is pushed to container registry
6. **Deploy**: Application is deployed to EKS using kubectl/Kubernetes manifests

## ✅ Testing

The deployment has been successfully tested:
- ✅ EKS cluster connectivity validated
- ✅ AWS credentials configured in Jenkins
- ✅ Nginx test deployment completed successfully

## 📂 Repository Structure

```
.
├── config.yaml           # Kubernetes config template for EKS access
├── useful-commands.md    # Setup commands for Jenkins server
└── README.md            # This file
```

## 🔐 Security Notes

- AWS credentials stored securely in Jenkins credentials store
- AWS IAM Authenticator used for cluster access
- Certificate-based authentication to EKS cluster
- Config file contains sensitive cluster information - keep secure

## 📝 Pipeline Flow

1. **Source**: Pull Java Maven application from jenkins-java-maven repository
2. **Branch**: Checkout `deploy-on-k8s` branch
3. **Build**: Maven compiles and packages the application
4. **Containerize**: Docker image is built
5. **Push**: Image pushed to container registry
6. **Deploy**: kubectl applies Kubernetes manifests to EKS cluster

## 🛠️ Useful Commands

### Verify Jenkins kubectl Access
```bash
kubectl get nodes
kubectl cluster-info
```

### Check Deployments
```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

### View Application Logs
```bash
kubectl logs -f deployment/<deployment-name>
```

### Update EKS kubeconfig Locally
```bash
aws eks update-kubeconfig --name <cluster-name> --region <region>
```

### Get Jenkins Container ID
```bash
docker ps | grep jenkins
```

## 📚 References

- Application Repository: [jenkins-java-maven](https://gitlab.com/twn-devops-bootcamp/latest/11-eks/jenkins-java-maven)
- Branch for deployment: `deploy-on-k8s`
- AWS IAM Authenticator: [GitHub](https://github.com/kubernetes-sigs/aws-iam-authenticator)

## 👤 Author

Daniel Nagorniy

---

**Note**: This is a configuration repository. The actual Java Maven application is in the jenkins-java-maven repository.
