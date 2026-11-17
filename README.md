# IS601 Module 10 — Secure FastAPI User Model and CI/CD

## Project Overview

This project extends a FastAPI calculator application by implementing secure user management with SQLAlchemy, Pydantic, and PostgreSQL. It includes password hashing, user data validation, automated testing, Docker containerization, and a CI/CD pipeline using GitHub Actions and Docker Hub.

## Technologies Used

- Python 3.11 and FastAPI
- SQLAlchemy and PostgreSQL 17
- Pydantic for data validation
- Password hashing and verification
- Pytest and Playwright for automated testing
- Docker and Docker Compose
- GitHub Actions for CI/CD
- Trivy for container security scanning
- Docker Hub for image deployment

## Secure User Model

The SQLAlchemy User model includes a username, email, hashed password, and creation timestamp.

Username and email fields have uniqueness constraints to prevent duplicate accounts. Passwords are hashed before storage rather than being saved as plaintext.

Pydantic schemas are used to validate incoming user data and serialize responses. The `UserCreate` schema accepts registration information, while `UserRead` excludes password hashes from returned user details.

## Running the Application Locally

### Requirements

Install Docker Desktop and Git.

Clone the repository:

```bash
git clone https://github.com/sandhuharnoor/IS601-Module10.git
cd IS601-Module10
```

### Environment Configuration

Copy the example environment file:

```powershell
Copy-Item .env.example .env

Start the application and PostgreSQL services:

```bash
docker compose up -d --build
```

Check the running containers:

```bash
docker compose ps
```

Access the application at:

http://localhost:8000

## Running Automated Tests Locally

With Docker Compose running, execute the full test suite:

```bash
docker compose exec -T web python -m pytest tests -v --cov=app --cov-report=term-missing
```

The project includes unit tests, PostgreSQL integration tests, and Playwright end-to-end tests.

The latest verified local test run completed with 75 passed tests, one skipped test, and 92% reported coverage.

**Note:** Tests that initialize or reset database tables should only run against a disposable test database.

## GitHub Actions CI/CD Pipeline

The project uses GitHub Actions to automate testing, security scanning, and deployment.

The workflow contains three jobs:

1. **Test:** Runs automated tests using a PostgreSQL service container and Playwright.
2. **Security:** Builds the Docker image and scans it with Trivy for HIGH and CRITICAL vulnerabilities.
3. **Deploy:** Builds and pushes the Docker image to Docker Hub after the preceding jobs succeed.

GitHub Actions repository secrets provide the credentials needed for application configuration and Docker Hub authentication.

The workflow runs on pushes and pull requests targeting the `main` branch. Docker Hub deployment occurs on successful pushes to `main`.

## Docker Hub Deployment

The published Docker image is available at:

https://hub.docker.com/r/hsandhu24/is601-module10

Pull the image:

```bash
docker pull hsandhu24/is601-module10:latest
```

The application requires an appropriately configured PostgreSQL database and environment variables to operate correctly. Docker Compose provides the local multi-container setup.

## Security Practices

- Password hashing before database storage
- Unique username and email constraints
- Pydantic input validation
- GitHub Actions secrets for sensitive configuration
- Non-root Docker container execution
- Automated Trivy vulnerability scanning

## Testing and Deployment Results

The complete local test suite passed with 75 passed tests and one skipped test. GitHub Actions workflow run #7 successfully completed the testing, security, and deployment jobs.

The Docker image was published to Docker Hub under the `latest` tag.

## Repository

GitHub: https://github.com/sandhuharnoor/IS601-Module10

Docker Hub: https://hub.docker.com/r/hsandhu24/is601-module10