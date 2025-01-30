# Containerized Sport API Configuration

## **Project Overview**
This project demonstrates building a containerized API management system for querying sports data. It leverages **Amazon ECS (Fargate)** for running containers, **Amazon API Gateway** for exposing REST endpoints, and an external **Sports API** for real-time sports data. The project showcases advanced cloud computing practices, including API management, container orchestration, and secure AWS integrations.

---

## **Features**
- Exposes a REST API for querying real-time sports data
- Runs a containerized backend using Amazon ECS with Fargate
- Scalable and serverless architecture
- API management and routing using Amazon API Gateway
 
---

## **Prerequisites**
- **Sports API Key**: Sign up for a free account and subscription & obtain your API Key at serpapi.com
- **AWS Account**: Create an AWS Account & have basic understanding of ECS, API Gateway, Docker & Python
- **AWS CLI Installed and Configured**: Install & configure AWS CLI to programatically interact with AWS
- **Serpapi Library**: Install Serpapi library in local environment "pip install google-search-results"
- **Docker CLI and Desktop Installed**: To build & push container images

---

## **Technologies**
- **Cloud Provider**: AWS
- **Core Services**: Amazon ECS (Fargate), API Gateway, CloudWatch
- **Programming Language**: Python 3.x
- **Containerization**: Docker
- **IAM Security**: Custom least privilege policies for ECS task execution and API Gateway

---

## **Project Structure**

```bash
sports-api-management/
├── app.py # Flask application for querying sports data
├── Dockerfile # Dockerfile to containerize the Flask app
├── requirements.txt # Python dependencies
├── .gitignore
└── README.md # Project documentation
```

---

## **Setup Instructions**

### **Clone the Repository**
```bash
git clone https://github.com/Faoziyah/30_days_devops_challenge.git
cd DevOps_Challenge_Day4/containerized-sports-api
```
### **Create ECR Repo**
```bash
aws ecr create-repository --repository-name sports-api --region us-east-1
```

### **Authenticate Build and Push the Docker Image**
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

docker build --platform linux/amd64 -t sports-api .
docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
```

### **Set Up ECS Cluster with Fargate**
# Create an ECS Cluster:
- Create a folder and name it (e.g. containerized-sport-api)
- Open it with vs code
- Clone the repository: git clone https://…
- Change directory to the folder: Cd  containerized-sport-api
- Create a repository in ECR with name and region: …
- Login to the ECR, change the Account ID to your AWS Account ID:…
- Build the image (the build process is stated in the Dockerfile): …
- Name the built image by tagging it
- Push the image to ECR

#Go to AWS Management console to confirm the ECR and the image

- In the console search for ECS in the search bar
- Choose Amazon Elastic Container Service (ECS) 
- Click on Cluster, then Create Cluster
- Name the cluster 
- Leave other filds as default 
- Click on Create cluster

---

- Click on task definitions
- Click on create new task definitions
- Name the task in the task definition family field (eg. Sport-api-task)
- Infrastructure Requirement: Choose AWS Fargate
- Scroll down to container 
- Name the container(e.g. sport-api-container)
- Copy and paste the uri of the ECR image to the Image URI field
- Container port: 8080
- Scroll down to Environment variables and input your environment variables sush as the SERP API and the SERP URL
- Scroll down and click Create Task Definition

---

- Click on Clusters
- Click on the Cluster we created 
- Click the Services Tab and click Create (to create a service)
- Scroll down to family and choose the task name
- Input the Service name (e.g. sport-api-service)
- Service type: Replica
- Desired tasks: 2
- Scroll down, under Networking, click Create new Security group
- Under Inbound rules for Security groups, Type: All TCP, Source: Anywhere
- Scroll down to Load balancing and choose Use Load Balancing
- Choose type as Application Load Balancer
- Choose the created Container 
- Name the load balancer (e.g. Sport-api-alb)
- Under Listener choose Create new listener. Port: 80, Protocol: HTTP
- Health check path: /sports
- Click on Create

---

- Go to EC2 dashboard to confirm the ALB is created 
- Copy the ALB DNS and paste it in your browser to view the app

---

- In the Management Console, Search for API Gateway in the search bar
- Choose API Gateway
- Click on Create API
- Choose REST API
- Name it (e.g. sport-api)
- Click create
- Click on create Resource
- Input the resource path: /sports and click Create
- Click on Create Method
- Type: GET
- Integration type: HTTP
- Turn on Http proxy integration
- HTTP method: GET
- Endpoint URL: copy and paste the endpoint of the ALB and add /sports
- Click  Create Method

---

- Click on Deploy API
- Stage: new stage
- State name: Dev
- Click on Deploy
- Copy the invoke url of the API and paste in your browser to confirm

---

- NB: Don’t forget to delete all created resources
- Delete Sport API
- Delete ALB
- Delete ECR
- Delete ECS (De-register the Task definition, click on the Cluster, click on update the service and change the replica from 2 to 0, Delete service, and Delete cluster)


### **What We Learned**
Setting up a scalable, containerized application with ECS
Creating public APIs using API Gateway.

### **Future Enhancements**
Add caching for frequent API requests using Amazon ElastiCache
Add DynamoDB to store user-specific queries and preferences
Secure the API Gateway using an API key or IAM-based authentication
Implement CI/CD for automating container deployments


