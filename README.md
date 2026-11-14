# University Student Portal

A containerized Flask application with Redis integration, managed using Docker Compose and deployed using Ansible.

## Technologies

- Python 3.12
- Flask 3.0.3
- Redis 7
- Docker
- Docker Compose
- Ansible
- Git

## Application Endpoints

- / - Portal homepage
- /health - Application health check
- /visits - Redis-backed visit counter

## Local Deployment

### Build the Docker image

    docker build -t student-portal:1.0 .

### Start the services

    docker compose up -d

### Access the application

    http://localhost:8080

## Project Structure

- app/ - Flask application
- ansible/ - Ansible deployment configuration
- Dockerfile - Application image definition
- compose.yml - Flask and Redis services
- README.md - Project documentation
