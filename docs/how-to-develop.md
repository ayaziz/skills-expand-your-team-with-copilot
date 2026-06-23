# Development Guide

## Initial Setup

This project is best developed using GitHub Codespaces, which provides a consistent development environment with all the necessary tools pre-configured.

### Setting up your development environment

1. Open the repository in a codespace
2. Wait for the container to finish building and installing dependencies
3. Install Python dependencies by running:

   ```bash
   pip install -r src/requirements.txt
   ```

### Dependencies

The project requires the following Python packages:

- FastAPI - Modern web framework for building APIs
- Uvicorn - ASGI server implementation for running the FastAPI application

These dependencies will be installed when you run `pip install -r src/requirements.txt`

## Docker Deployment

### Running with Docker Compose

The project includes Docker and Docker Compose configuration for containerized local development and deployment.

#### Prerequisites
- Docker and Docker Compose installed on your system

#### Quick Start

1. Build and start the services:

   ```bash
   docker-compose up --build
   ```

2. The application will be available at:
   - Web API: http://localhost:8000
   - API documentation: http://localhost:8000/docs
   - MongoDB: localhost:27017

3. To stop the services:

   ```bash
   docker-compose down
   ```

#### Development with Docker Compose

The Docker Compose configuration includes hot-reload for development:

```bash
# Start services with auto-reload
docker-compose up

# View logs
docker-compose logs -f web

# Stop services
docker-compose down
```

#### Production Deployment

For production deployment, you can use the Dockerfile directly:

```bash
docker build -t mergington-api .
docker run -p 8000:8000 -e MONGODB_URI=<production-db-uri> mergington-api
```

## Debugging

### Running the website locally

1. From VS Code's Run and Debug view (Ctrl+Shift+D), select "Launch Mergington WebApp" from the launch configuration dropdown
2. Press F5 or click the green play button to start debugging
3. The website will be available at `http://localhost:8000`
4. The API documentation will be available at `http://localhost:8000/docs`

### Debugging tips

- FastAPI's auto-reload feature will automatically restart the server when you make code changes
- Use the interactive API documentation at `/docs` to test your endpoints

## Usage

### Running the application

1. Install the dependencies:

   ```bash
   pip install -r src/requirements.txt
   ```

2. Run the application:

   ```bash
   python -m uvicorn src.app:app --host 0.0.0.0 --port 8000
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

### API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Sign up for an activity                                             |

> [!IMPORTANT]
> All data is stored in memory, which means data will be reset when the server restarts.
