---

# Django App API

This project is a Django-based web application, which can be deployed locally using Docker. Follow the steps below to get the project up and running.

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
