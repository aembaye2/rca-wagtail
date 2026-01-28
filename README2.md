# RCA Wagtail Development Setup Guide

This guide outlines the steps to set up and run the RCA Wagtail project after pulling the latest changes from the repository.

## Prerequisites

- Docker (version 19.0.0 or later)
- Docker Compose (version 1.24.0 or later)
- Git
- VS Code with Dev Containers extension (recommended)

## Quick Setup with VS Code Dev Containers (Recommended)

If you're using VS Code with the Dev Containers extension, simply open the project folder. VS Code will automatically:

1. Detect the `.devcontainer` configuration
2. Build and start the Docker containers (PostgreSQL, Redis, and web service)
3. Open the workspace inside the running container

Then proceed to **Step 3: Set Up the Database** below.

## Manual Setup (Alternative)

If you prefer manual control or aren't using VS Code:

### 1. Clone or Pull the Repository

```bash
git clone https://github.com/aembaye2/rca-wagtail.git
cd rca-wagtail
```

### 2. Build and Start the Dev Container

Pull the latest Docker images and build the containers:

```bash
docker-compose pull
docker-compose build
```

Start the services (PostgreSQL, Redis, and web):

```bash
docker-compose up -d
```

### 3. Set Up the Database

Pull the latest Docker images and build the containers:

```bash
docker-compose pull
docker-compose build
```

Start the services (PostgreSQL, Redis, and web):

```bash
docker-compose up -d
```

### 3. Set Up the Database

Run Django migrations to set up the database schema:

```bash
docker-compose exec web python manage.py migrate
```

Create the Django cache table:

```bash
docker-compose exec web python manage.py createcachetable
```

### 4. Run Data Anonymization (Birdbath)

For development safety, run the Birdbath command to anonymize any sensitive data:

```bash
docker-compose exec web python manage.py run_birdbath
```

### 5. Build Frontend Assets

Compile the JavaScript and CSS assets:

```bash
docker-compose exec web bash --login -c 'npm run build:prod'
```

### 6. Start the Development Server

Launch the Django development server:

```bash
docker-compose exec -d web python manage.py runserver 0.0.0.0:8000
```

### 7. Access the Application

- **Web Application**: http://localhost:8000/
- **Admin Interface**: http://localhost:8000/admin/
- **Wagtail CMS**: http://localhost:8000/cms/
- **Documentation**: http://localhost:8001/ (mkdocs serve)

## Additional Commands

### Stop the Services

```bash
docker-compose stop
```

### View Logs

```bash
docker-compose logs web
```

### Access the Container Shell

```bash
docker-compose exec web bash
```

### Create a Superuser (if needed)

```bash
docker-compose exec web python manage.py createsuperuser
```

## Troubleshooting

- If you encounter issues with static files, ensure the frontend build completed successfully.
- For database connection issues, verify PostgreSQL and Redis containers are running.
- Check the logs with `docker-compose logs` for detailed error messages.

## Notes

- The project uses Python 3.11, Node.js 20, PostgreSQL 15, and Redis 7.2.
- All services run in Docker containers for consistent development environments.
- The dev container includes nvm for Node.js management.