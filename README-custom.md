# Custom Airflow Docker Image

This branch contains customizations to build a custom Apache Airflow Docker image with specific requirements.

## Changes Made

The main change is the addition of a GitHub Actions workflow file: `.github/workflows/my-image-build.yml`

### Custom Image Configuration

The workflow builds a custom Airflow image with the following specifications:

- **Airflow Version**: 2.6.2
- **Python Base Image**: python:3.9-slim-bullseye
- **Target Platforms**: linux/amd64, linux/arm64
- **Timezone**: Europe/London

### Airflow Extras Included

The image includes the following Airflow extras:
- `async` - Async operators support
- `cncf.kubernetes` - Kubernetes executor and operators
- `dask` - Dask distributed computing
- `docker` - Docker operators
- `grpc` - gRPC support
- `http` - HTTP operators
- `postgres` - PostgreSQL support
- `statsd` - StatsD metrics
- `virtualenv` - Python virtual environment support

### Additional Runtime Dependencies

- `git` - Version control system
- `sshpass` - SSH password authentication

### Build Triggers

The workflow can be triggered by:
- **Push events** - Automatic build on any push to the branch
- **Manual dispatch** - Manual trigger via GitHub Actions UI

### Docker Hub Publishing

Images are published to Docker Hub with the tag:
```
<your-dockerhub-username>/airflow:latest
```

## Usage

### Building Locally

To build the image locally:
```bash
docker build \
  --build-arg AIRFLOW_VERSION=2.6.2 \
  --build-arg AIRFLOW_EXTRAS=async,cncf.kubernetes,dask,docker,grpc,http,postgres,statsd,virtualenv \
  --build-arg PYTHON_BASE_IMAGE=python:3.9-slim-bullseye \
  --build-arg INSTALL_MYSQL_CLIENT=false \
  --build-arg INSTALL_MSSQL_CLIENT=false \
  --build-arg ADDITIONAL_RUNTIME_APT_DEPS="git sshpass" \
  --build-arg TZ=Europe/London \
  -t your-username/airflow:latest .
```

### Pulling from Docker Hub

Once built via GitHub Actions:
```bash
docker pull <your-dockerhub-username>/airflow:latest
```

### Running the Container

```bash
docker run -d -p 8080:8080 <your-dockerhub-username>/airflow:latest
```

## Requirements

To use the GitHub Actions workflow, ensure you have the following secrets configured in your GitHub repository:
- `DOCKERHUB_USERNAME` - Your Docker Hub username
- `DOCKERHUB_TOKEN` - Your Docker Hub access token

## Rationale

This custom image was created to:
1. Include specific Airflow extras needed for the project
2. Add runtime dependencies (`git`, `sshpass`) required by DAGs
3. Set appropriate timezone (Europe/London)
4. Support both AMD64 and ARM64 architectures
5. Provide a consistent, reproducible Airflow environment