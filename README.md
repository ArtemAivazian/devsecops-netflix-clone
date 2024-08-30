# DevSecOps pipeline for the Netflix clone
![alt text](assets/project_arch.png)

## Problems This Project Solves

This project addresses several key challenges faced in modern software development and deployment:

1. **Automated Deployment**: The project automates the process of deploying a Netflix clone application to the cloud, reducing manual intervention and human error.
2. **Scalability**: By deploying the application in a containerized environment and later moving to a Kubernetes cluster, the project ensures the application can easily scale to handle increased traffic.
3. **Security**: Through the integration of SonarQube and Trivy, the project embeds security checks into the CI/CD pipeline, ensuring that vulnerabilities are identified and addressed before deployment.
4. **Continuous Integration and Delivery**: The Jenkins pipeline enables continuous integration and delivery (CI/CD), allowing for frequent and reliable code deployments with minimal downtime.
5. **Infrastructure Monitoring**: The use of Prometheus and Grafana for monitoring ensures that the infrastructure's health is continuously tracked, with alerts set up to notify of any potential issues, ensuring high availability.

## What This Project Learns

This project provides hands-on experience with deploying a scalable, secure, and monitored web application in a cloud environment using DevSecOps practices. Through this project, participants will learn:

1. **Cloud Infrastructure Management**: Provisioning and managing AWS EC2 instances with Ubuntu 22.04 to host applications.
2. **Containerization with Docker**: Building and running applications inside Docker containers, providing isolated environments that simplify deployment and scalability.
3. **CI/CD Automation with Jenkins**: Setting up Jenkins pipelines to automate the entire deployment process, including code integration, testing, and deployment.
4. **Security and Vulnerability Scanning**: Integrating security tools like SonarQube and Trivy into the CI/CD pipeline to automatically scan code and Docker images for vulnerabilities.
5. **Monitoring and Alerting**: Implementing monitoring solutions using Prometheus and Grafana to visualize application performance and set up alerts for potential issues.
6. **Kubernetes Management**: Deploying applications in a Kubernetes cluster, managing node groups, and using Helm for system-level monitoring.
7. **Notification and Incident Management**: Configuring notification services to alert the team in case of failures or issues in the deployment process.


### **Phase 1: Initial Setup and Deployment**

**Step 1: Launch EC2 (Ubuntu 22.04):**

![alt text](assets/jenkins_server.png)

- Provision an EC2 instance on AWS with Ubuntu 22.04.
- Connect to the instance using SSH.

**Step 2: Clone the Code:**

- Update all the packages and then clone the code.
- Clone your application's code repository onto the EC2 instance:
    
    ```bash
    git clone https://github.com/N4si/DevSecOps-Project.git
    ```
    

**Step 3: Install Docker and Run the App Using a Container:**

- Set up Docker on the EC2 instance:
    
    ```bash
    
    sudo apt-get update
    sudo apt-get install docker.io -y
    sudo usermod -aG docker $USER  # Replace with your system's username, e.g., 'ubuntu'
    newgrp docker
    sudo chmod 777 /var/run/docker.sock
    ```
    
- Build and run your application using Docker containers:
    
    ```bash
    docker build -t netflix .
    docker run -d --name netflix -p 8081:80 netflix:latest
    
    #to delete
    docker stop <containerid>
    docker rmi -f netflix
    ```

It will show an error cause you need API key:

![alt text](<assets/Screenshot 2024-08-30 at 14.13.20.png>)

**Step 4: Get the API Key:**

- Open a web browser and navigate to TMDB (The Movie Database) website.
- Click on "Login" and create an account.
- Once logged in, go to your profile and select "Settings."
- Click on "API" from the left-side panel.
- Create a new API key by clicking "Create" and accepting the terms and conditions.
- Provide the required basic details and click "Submit."
- You will receive your TMDB API key.

Now recreate the Docker image with your api key:
```
docker build --build-arg TMDB_V3_API_KEY=<your-api-key> -t netflix .
```
And run a container and get:
![alt text](assets/pass_api_key.png)

