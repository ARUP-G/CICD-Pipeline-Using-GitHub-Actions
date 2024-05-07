## Boardgame SpringBoot Application on Kubernetes with Monitoring and Security

This project provides a streamlined approach to deploy your SpringBoot boardgame application on a Kubernetes cluster hosted on AWS while ensuring comprehensive monitoring and security. The deployment process is automated using a GitHub Actions CICD pipeline.

## Overview

![Steps](images/Github%20actions.png)

- **Backend**: SpringBoot application for your boardgame.
- **Cloud Provider**: Amazon Web Services.
- **Containerization**: Docker for building container images.
- **Code Quality Check**: SonarQube for code quality check.
- **Orchestration**: Kubernetes for managing container deployments and scaling.
- **Self-Hosted Runner**: Dedicated EC2 instance for running GitHub Actions workflows
#### Monitoring
- **Prometheus**: Collects and stores application and system metrics.
- **Grafana**: Visualizes collected metrics for insights.
- **Node Exporter**: Exposes system stats as Prometheus metrics.
- **Blackbox Exporter**: Monitors external services.
#### **Security**
- **Trivy**: Scans container images for vulnerabilities.
- **CI/CD**: GitHub Actions workflow automates build, image push, and deployment.

## Features

- Automated build and deployment process through GitHub Actions
- Containerized application for portability and scalability
- Real-time monitoring of application and system health
- Customizable dashboards for visualizing key metrics
- vulnerability scanning of container images with Trivy
## Prerequisites

- Java 11+
- AWS Account with IAM user access
- kubectl configured to interact with your Kubernetes cluster
- Git installed
- Docker installed for 

## Setting Up the Project

### Clone the Repository:

```Bash
git clone https://github.com/ARUP-G/CICD-Pipeline-Using-GitHub-Actions.git
```

### Configure AWS :
We are creating 2 Ec2 instances for kubernetes Master and Kubernetes Worker and another Ec2 instance for Github Actions Self-Hosted Runner.
To create an Ubuntu EC2 instance in AWS, follow these steps:

1. **Sign in to the AWS Management Console**:
   - Go to the AWS Management Console at https://aws.amazon.com/console/.
   - Sign in with your AWS account credentials.

2. **Navigate to EC2**:
   - Once logged in, navigate to the EC2 dashboard by typing "EC2" in the search bar at the top or by selecting "Services" and then "EC2" under the "Compute" section.

3. **Launch Instance**:
   - Click on the "Instances" link in the EC2 dashboard sidebar.
   - Click the "Launch Instance" button.

4. **Choose an Amazon Machine Image (AMI)**:
   - In the "Step 1: Choose an Amazon Machine Image (AMI)" section, select "Ubuntu" from the list of available AMIs.
   - Choose the Ubuntu version you want to use. For example, "Ubuntu Server 20.04 LTS".
   - Click "Select".

5. **Choose an Instance Type**:
   - In the "Step 2: Choose an Instance Type" section, select the instance type that fits your requirements. The default option (usually a t2.micro instance) is suitable for testing and small workloads.
   - Click "Next: Configure Instance Details".
   ```
   Note: Try to select instance type which has 2 or more Cpu to omit any error on K8s side.
   ```

6. **Configure Instance Details**:
   - Optionally, configure instance details such as network settings, subnets, IAM role, etc. You can leave these settings as default for now.
   - Click "Next: Add Storage".

7. **Add Storage**:
   - Specify the size of the root volume (default is usually fine for testing purposes).
   - Click "Next: Add Tags".

8. **Add Tags**:
   - Optionally, add tags to your instance for better organization and management.
   - Click "Next: Configure Security Group".

9. **Configure Security Group**:
   - In the "Step 6: Configure Security Group" section, configure the security group to allow SSH access (port 22) from your IP address.
   - You may also want to allow other ports based on your requirements (e.g., HTTP, HTTPS) as in this pic ![Alt text](images/inbound-rules.png)
   - Click "Review and Launch".

10. **Review and Launch**:
    - Review the configuration of your instance.
    - Click "Launch".

11. **Select Key Pair**:
    - In the pop-up window, select an existing key pair or create a new one.
    - Check the acknowledgment box.
    - Click "Launch Instances".

12. **Access Your Instance**:
    - Use Mobaxterm 


### Configure Kubernetes Cluster:
1. Update System Packages [On Master & Worker Node]

```bash
sudo apt-get update
```

2. **Install Docker[On Master & Worker Node]**

```bash
sudo apt install docker.io -y
sudo chmod 666 /var/run/docker.sock
```

3. **Install Required Dependencies for Kubernetes[On Master & Worker Node]**

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
sudo mkdir -p -m 755 /etc/apt/keyrings
```

4. **Add Kubernetes Repository and GPG Key[On Master & Worker Node]**

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

5. **Update Package List[On Master & Worker Node]**

```bash
sudo apt update
```

6. **Install Kubernetes Components[On Master & Worker Node]**

```bash
sudo apt install -y kubeadm=1.28.1-1.1 kubelet=1.28.1-1.1 kubectl=1.28.1-1.1
```

7. **Initialize Kubernetes Master Node [On MasterNode]**

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

8. **Configure Kubernetes Cluster [On MasterNode]**

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

9. **Deploy Networking Solution (Calico) [On MasterNode]**
```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

10. **Deploy Ingress Controller (NGINX) [On MasterNode]**

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.49.0/deploy/static/provider/baremetal/deploy.yaml
```
11. **Connect Master Node with Worker Node**

### Configure GitHub Actions:

- Create a .github/workflows directory in your repository.
- Create a build-and-deploy.yml file within the directory. This YAML file will define the CICD pipeline workflow.

