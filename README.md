# University Student Portal

A containerized Flask application integrated with Redis, packaged using Docker and deployed to a remote RHEL server using Ansible.

## Architecture

The project uses two RHEL 9 virtual machines:

- **Control Node:** Runs Ansible and manages the deployment.
- **App Server:** Hosts Docker Engine, the Flask application, and Redis.

Ansible connects to the App Server over SSH, installs Docker, transfers application files, builds the Docker image, and starts services using Docker Compose.

## Technologies

- Python 3.12
- Flask 3.0.3
- Redis 7
- Docker Engine
- Docker Compose
- Ansible Core
- Git

## Application Endpoints

| Endpoint | Description |
|---|---|
| `/` | Portal homepage |
| `/health` | Application health check |
| `/visits` | Redis-backed visit counter |

## Project Structure

- `app/` - Flask application and requirements
- `ansible/` - Ansible inventory and deployment playbook
- `Dockerfile` - Application image definition
- `compose.yml` - Flask and Redis services
- `.gitignore` - Excludes sensitive and unnecessary files

## Local Deployment

Build the Docker image:

    docker build -t student-portal:1.0 .

Start the services:

    docker compose up -d

Access the application:

    http://localhost:8080

## Ansible Deployment

### Prerequisites

- Ansible installed on the Control Node.
- SSH connectivity to the App Server.
- Passwordless SSH authentication configured.

### Test connectivity

    ansible -i ansible/inventory.ini app_servers -m ping -K

### Run the deployment

    ansible-playbook -i ansible/inventory.ini ansible/deploy.yml -K

The playbook:

1. Downloads Docker repository configuration.
2. Installs container SELinux policies.
3. Installs Docker Engine and Docker Compose.
4. Enables and starts Docker.
5. Creates the application directory.
6. Transfers application files to the App Server.
7. Builds the Docker image.
8. Starts Flask and Redis using Docker Compose.
9. Verifies the application health endpoint.

## Verification

Test the application from the Control Node:

    curl http://192.168.145.130:8080/
    curl http://192.168.145.130:8080/health
    curl http://192.168.145.130:8080/visits

Expected health response:

    {"status":"healthy"}

## Redis Data Persistence

Redis uses a named Docker volume called `redis-data`.

Restart Redis:

    sudo docker compose restart redis

Check the stored visit counter:

    sudo docker compose exec redis redis-cli GET visits

The previously stored counter value should remain available after the restart.

## Ansible Idempotency

Run the playbook again:

    ansible-playbook -i ansible/inventory.ini ansible/deploy.yml -K

The second execution completed with `changed=0` and `failed=0`, demonstrating idempotency.

## Version Control Workflow

The project was managed using Git and GitHub:

- `dev` - Initial application and Docker development.
- `ansible-deployment` - Ansible deployment automation.
- `main` - Final integrated project.

Changes were merged into `main` through Pull Requests.
