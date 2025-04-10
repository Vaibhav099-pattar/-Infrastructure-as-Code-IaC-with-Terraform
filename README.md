# Terraform Docker Nginx Setup on RedHat

This guide will walk you through the process of setting up a Docker container running Nginx using Terraform on a RedHat-based system. 

## Prerequisites

Before proceeding, ensure the following are installed on your RedHat system:

- **Terraform**: Used for provisioning infrastructure as code.
- **Docker**: For containerization.
  
### Step 1: Install Docker
1. **Install Docker**:
    - Follow the instructions on Docker's official website or use the command:
    
      ```bash
      sudo dnf install -y docker-ce docker-ce-cli containerd.io
      sudo systemctl start docker
      sudo systemctl enable docker
      ```
   
2. **Start Docker service**:
    ```bash
    sudo systemctl start docker
    ```

3. **Verify Docker installation**:
    ```bash
    docker --version
    docker ps
    ```

### Step 2: Install Terraform

1. **Install Terraform**:
    - Follow the official Terraform installation guide or use the command:
    
      ```bash
      sudo dnf install -y yum-utils
      sudo dnf config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
      sudo dnf install terraform
      ```

2. **Verify Terraform installation**:
    ```bash
    terraform --version
    ```

### Step 3: Create the `main.tf` file

1. **Create a file** called `main.tf` with the following content:

```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.2"
    }
  }
}

provider "docker" {
  host = "unix:///var/run/docker.sock"
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx_container" {
  name  = "nginx_terraform"
  image = docker_image.nginx.name 
  ports {
    internal = 80
    external = 8081  
  }
}
Step 4: Initialize Terraform :terraform init
Step 5: Plan the Terraform Execution:terraform plan
Step 6: Apply the Terraform Configuration:terraform apply
Step 7: Verify the Docker Container:docker ps:http://localhost:8081:http://0.0.0.0:8081/



