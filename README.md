---

# Django Recipe App API

This project is a Django-based web application that can be deployed locally using Docker or on an AWS EC2 instance. Follow the steps below to get the project up and running.

## Prerequisites

Ensure you have the following tools installed on your local machine:

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Steps to Deploy Locally

### 1. Clone the Project

Clone the project from GitHub using the following command:

```bash
git clone https://github.com/sonvt8/django-app-api.git
```

### 2. Create a `.env` File

Create a `.env` file in the project root directory based on the `.env.example` template. The `.env.example` file contains the required environment variables. You can copy the content and modify it with the necessary values.

```bash
cp .env.example .env
```

Fill in the required parameters in the `.env` file. Make sure to add any sensitive or project-specific values (e.g., database connection details, secret keys, etc.).

### 3. Build the Docker Containers

Run the following command to build the Docker containers:

```bash
docker-compose build
```

### 4. Run Tests and Linting

Before proceeding, ensure that all tests pass and the code quality checks pass (using `flake8` for linting). Run the following command:

```bash
docker-compose run --rm app sh -c "python manage.py test && flake8"
```

If all tests pass without errors, you are good to go.

### 5. Create a Superuser

Create a superuser for accessing the Django admin panel by running the following command:

```bash
docker-compose run --rm app sh -c "python manage.py createsuperuser"
```

You will be prompted to enter a username, email, and password for the superuser.

### 6. Start the Application

Now, start the application using the following command:

```bash
docker-compose up
```

This will start all the services defined in the `docker-compose.yml` file.

### 7. Access the Application

Once the containers are up and running, you can access the application in your browser:

- **Django Admin**: [http://localhost:8000/admin](http://localhost:8000/admin)
  - Login with the superuser credentials you created earlier.
  
- **API Documentation**: [http://localhost:8000/api/doc](http://localhost:8000/api/doc)
  - Explore the API documentation provided by the project.

### 8. Stopping the Application

To stop the Docker containers, press `CTRL+C` or run the following command:

```bash
docker-compose down
```

This will stop and remove the running containers.

---

## Additional Notes

- Make sure to set up your `.env` file correctly with valid credentials to avoid any runtime issues.
- The `docker-compose.yml` file defines all the necessary services to run the project, including the web application and any databases or caching services.

Now you should have everything you need to deploy and run the project locally on your machine.

---


## AWS EC2 Deployment

### 1. Launch an EC2 Instance

1. Sign in to your AWS Management Console.
2. Navigate to **EC2** and click on **Launch Instance**.
3. Choose the **Amazon Linux 2** AMI or your preferred Linux distribution.
4. Select an instance type (e.g., `t2.micro` for free tier).
5. Ensure the instance has security groups allowing **SSH (port 22)** and **HTTP (port 80)** access.
6. Launch the instance and download the `.pem key` for SSH access.

### 2. SSH into Your EC2 Instance

Once your EC2 instance is running, SSH into it from your local machine:

```bash
ssh -i "path_to_your_key.pem" ec2-user@your_instance_public_ip
```

### 3. Update and Install Required Packages

Once connected, update the system and install Docker:

```bash
sudo yum update -y
sudo yum install docker -y
```

Start Docker and add the user to the Docker group:

```bash
sudo service docker start
sudo usermod -a -G docker ec2-user
```

### 4. Install Docker Compose

Install Docker Compose by downloading the binary:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Check if Docker Compose is installed:

```bash
docker-compose --version
```

### 5. Clone the Project to EC2

Clone the project to your EC2 instance:

```bash
git clone https://github.com/sonvt8/django-app-api.git
cd django-app-api
```

### 6. Set Up Environment Variables

Create a `.env` file in the project root directory:

```bash
cp .env.example .env
```

Edit the `.env` file with your production environment settings like database credentials and secret keys.

### 7. Build and Run the Docker Containers

Build the Docker containers:

```bash
docker-compose -f docker-compose-deploy.yml build
```

Run the application:

```bash
docker-compose -f docker-compose-deploy.yml up -d
```

### 8. Create a Superuser

Create a superuser for accessing the admin interface:

```bash
docker-compose -f docker-compose-deploy.yml run --rm app sh -c "python manage.py createsuperuser"
```

### 9. Access the Application

Once the containers are running, you can access your app via:

- **Django Admin**: `http://<your-ec2-instance-public-ip>/admin`
  - Use the superuser credentials you created earlier.
  
- **API Documentation**: `http://<your-ec2-instance-public-ip>/api/doc`

---

## Additional Notes

- Make sure your EC2 security group allows access to necessary ports, such as 8000 and 80 for HTTP.
- Keep your `.env` file updated with production-level settings when deploying to production environments.

---

## Updating the Application

When you push new versions of your code and want to update the service on your server, follow these steps:

#### Step 1: Pull the Latest Changes

First, pull the latest changes from the Git repository to your server:

```bash
git pull origin
```

#### Step 2: Rebuild the Application Image

Rebuild the Docker image so it includes the latest code changes:

```bash
docker-compose -f docker-compose-deploy.yml build app
```

#### Step 3: Restart the Application

Apply the update by restarting the app container. Use the following command to restart only the `app` service without affecting its dependencies (like the database or proxy):

```bash
docker-compose -f docker-compose-deploy.yml up --no-deps -d app
```

The `--no-deps` flag ensures that dependent services (such as a database or proxy) will not restart, and only the `app` container will be updated and restarted.

---

With this updated guide, the application can be deployed, maintained, and updated on an online host with Docker Compose.
