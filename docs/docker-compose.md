# Flask-MySQL Docker Project

## Overview
This project is a Flask-based web application using MySQL as the database. The application is containerized using Docker and managed via Docker Compose.

## Project Structure
```
.
├── app
│   ├── app.py                # Flask application entry point
│   ├── Dockerfile.app        # Dockerfile for the app container
│   ├── init.sql              # SQL script to initialize the database
│   ├── requirements.txt      # Python dependencies
│   └── templates
│       └── index.html        # HTML template for the app
├── docker-compose.yml        # Docker Compose configuration
├── docs                      # Documentation files
│   ├── app.md                # Flask app documentation
│   ├── docker-compose.md     # Docker Compose setup documentation
│   └── Dockerfile.md         # Dockerfile details
└── README.md                 # Project documentation
```

## Prerequisites
Ensure you have the following installed:
- Docker
- Docker Compose

## Setup & Installation

### 1️⃣ Clone the Repository
```sh
git clone <repo-url>
cd <repo-folder>
```

### 2️⃣ Build and Run Containers
```sh
docker compose up --build
```
This will build and start the Flask app and MySQL database containers.

### 3️⃣ Verify the Running Containers
```sh
docker ps
```
You should see two running containers: one for the Flask app and one for MySQL.

## Docker Compose Configuration

The `docker-compose.yml` file defines the services, networks, and volumes used in this project.

### Services
1. **App Service (`app`)**:
   - Builds the Flask application using `Dockerfile.app`.
   - Connects to `app_network`.
   - Exposes port `5000`.
   - Depends on the `db` service, ensuring it starts only when the database is healthy.

2. **Database Service (`db`)**:
   - Uses the latest MySQL image.
   - Sets up the MySQL database with predefined credentials.
   - Mounts a persistent volume `db_data` for database storage.
   - Executes `init.sql` on startup to initialize the database.
   - Includes a health check to verify MySQL availability.

### Networks
- `app_network`: A bridge network for communication between the app and database services.

### Volumes
- `db_data`: A named volume for persistent MySQL data storage.

## Access the Application
- Open a web browser and go to: `http://localhost:5000`

## Stopping the Application
To stop and remove the containers, run:
```sh
docker compose down
```

## Database Setup
The `init.sql` script is executed automatically when the MySQL container starts. You can modify this file to customize your database setup.

## Logs & Debugging
- View application logs:
  ```sh
  docker compose logs app
  ```
- Access the running container:
  ```sh
  docker exec -it <container_id> sh
  ```

## Future Enhancements
- Add authentication to the Flask app
- Use a `.env` file for environment variables
- Implement CI/CD pipeline for automatic deployment
