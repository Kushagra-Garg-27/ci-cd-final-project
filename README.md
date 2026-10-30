# CI/CD Final Project - Hit Counter Service

[![CI workflow](https://github.com/Kushagra-Garg-27/ci-cd-final-project/actions/workflows/workflow.yml/badge.svg)](https://github.com/Kushagra-Garg-27/ci-cd-final-project/actions/workflows/workflow.yml)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

## Project Overview

This repository contains the completed final project for the **Continuous Integration and Continuous Delivery (CI/CD)** course on Coursera, part of the **IBM DevOps and Software Engineering Professional Certificate**.

The project delivers a production-ready, cloud-native RESTful hit counter microservice built using Python and Flask. It demonstrates an automated CI/CD lifecycle spanning:
1. **Automated Continuous Integration via GitHub Actions** (linting with `flake8` and testing with `nose`).
2. **Cloud-Native Continuous Delivery via Tekton / OpenShift Pipelines** (tasks for workspace cleanup and automated test execution).
3. **Containerization with Docker** (multi-stage non-root container deployment using Gunicorn).
4. **OpenShift Cluster Deployment & Pipeline Orchestration** (PVC storage, pipeline execution, and live pod health checks).

---

## Technology Stack

- **Application Framework:** Python 3.9, Flask 2.1.2, Werkzeug 2.1.2
- **WSGI Production Server:** Gunicorn 20.1.0
- **Code Linter:** Flake8 4.0.1 (PEP 8 compliance, max-line-length 127)
- **Unit Testing Framework:** Nose 1.3.7, Pinocchio 0.4.3, Coverage 6.3.2
- **Continuous Integration:** GitHub Actions (`.github/workflows/workflow.yml`)
- **Pipeline Orchestration:** Tekton Pipelines / OpenShift Pipelines (`.tekton/tasks.yml`, `.tekton/pipeline.yml`)
- **Containerization:** Docker (`python:3.9-slim` base, non-root user `service`, port 8000)
- **Target Platform:** Red Hat OpenShift Container Platform / Kubernetes

---

## Project Structure

```text
ci-cd-final-project/
├── .github/
│   └── workflows/
│       └── workflow.yml      # GitHub Actions CI workflow definition
├── .tekton/
│   ├── tasks.yml             # Tekton Task resources: cleanup and nose
│   ├── pipeline.yml          # OpenShift Pipeline: oc-pipelines-oc-final
│   └── pvc.yml               # PersistentVolumeClaim for pipeline workspace
├── bin/
│   └── setup.sh              # Environment setup script
├── service/
│   ├── __init__.py           # Flask app factory and initialization
│   ├── routes.py             # REST API endpoints (/health, /counters)
│   └── common/
│       ├── error_handlers.py # HTTP error response handlers
│       ├── log_handlers.py   # Application logging setup
│       └── status.py         # HTTP status code constants
├── tests/
│   └── test_routes.py        # Complete REST API unit test suite
├── Dockerfile                # Production container image definition
├── Procfile                  # Process manager declaration
├── requirements.txt          # Python application & tool dependencies
├── setup.cfg                 # Test runner & flake8 linter configuration
├── .gitignore                # Git ignore patterns
└── README.md                 # Project documentation
```

---

## Local Development & Setup

### 1. Prerequisites
- Python 3.9 (or virtual environment with Python 3.9)
- Git

### 2. Install Dependencies

Create and activate a virtual environment, then install dependencies:

```bash
# Create virtual environment
python3.9 -m venv .venv

# Activate virtual environment
# On Linux/macOS:
source .venv/bin/activate
# On Windows (PowerShell):
.\.venv\Scripts\Activate.ps1

# Upgrade pip and install requirements
pip install --upgrade pip wheel
pip install -r requirements.txt
```

### 3. Run the Application

Start the development server using Flask:

```bash
python -m flask run --host=0.0.0.0 --port=8000
```

Or run using the production WSGI server (Gunicorn):

```bash
gunicorn service:app --bind 0.0.0.0:8000
```

---

## Running Linting and Unit Tests

### Code Quality (Flake8)
Verify that the codebase adheres to PEP 8 standards:

```bash
flake8 .
```

Configuration in `setup.cfg` ensures compliance with line-length constraints (`max-line-length = 127`) and appropriate per-file ignores for package `__init__.py` files.

### Unit Testing (Nose)
Execute the automated test suite with spec output and code coverage:

```bash
nosetests
```

The test suite runs with 100% pass rate against all routes, validating index, health check, counter creation, counter collision, retrieval, update, and deletion endpoints.

---

## Health-Check Endpoint & REST API

### Health Check Endpoint
- **URL:** `/health`
- **Method:** `GET`
- **Response Code:** `200 OK`
- **Response Payload:**
  ```json
  {
    "status": "OK"
  }
  ```

### Counter Endpoints
| Method | Endpoint | Description | Expected Status |
|---|---|---|---|
| `GET` | `/` | Root info & service metadata | `200 OK` |
| `GET` | `/health` | Application health check | `200 OK` |
| `GET` | `/counters` | List all active counters | `200 OK` |
| `POST` | `/counters/<name>` | Create a new counter | `201 Created` |
| `GET` | `/counters/<name>` | Read counter value | `200 OK` |
| `PUT` | `/counters/<name>` | Increment counter | `200 OK` |
| `DELETE` | `/counters/<name>` | Remove counter | `204 No Content` |

---

## Containerization (Docker)

Build the Docker image locally:

```bash
docker build -t counter-service:latest .
```

Run the container:

```bash
docker run -d --name counter-service -p 8000:8000 counter-service:latest
```

Verify health check:

```bash
curl -i http://localhost:8000/health
```

---

## GitHub Actions CI Workflow

The CI workflow is defined in `.github/workflows/workflow.yml`. It is triggered on:
- Every `push` to `main` or `master` branches.
- Every `pull_request` targeting `main` or `master`.

### Workflow Stages:
1. **Checkout:** Checks out repository code via `actions/checkout@v3`.
2. **Set up Python:** Configures Python 3.9 via `actions/setup-python@v4`.
3. **Install dependencies:** Upgrades pip and installs packages from `requirements.txt`.
4. **Lint with flake8:** Executes `flake8 .` to enforce PEP 8 styling.
5. **Run unit tests with nose:** Executes `nosetests` to verify all unit tests and generate coverage reports.

---

## Tekton / OpenShift Pipelines

The project includes cloud-native CI/CD manifests located in `.tekton/`:

### 1. Tasks Definition (`.tekton/tasks.yml`)
- **`cleanup` Task:** Runs an Alpine container (`alpine:3.19`) that cleans temporary build and test files (`.coverage`, `unittests.xml`, `__pycache__`) and prepares the shared workspace for a fresh clone.
- **`nose` Task:** Runs a Python 3.9 container (`python:3.9-slim`) that installs dependencies from `requirements.txt` and executes `nosetests -v` against the shared workspace.

### 2. Pipeline Definition (`.tekton/pipeline.yml`)
- **Pipeline Name:** `oc-pipelines-oc-final`
- **Workspaces:** Uses `pipeline-workspace` backed by a PersistentVolumeClaim (`pipeline-pvc`).
- **Sequential Pipeline Tasks:**
  1. `cleanup`: Cleans workspace.
  2. `clone`: Clones source repository into `pipeline-workspace` (`git-clone`).
  3. `lint`: Runs flake8 linter.
  4. `tests`: Runs unit tests via custom `nose` task.

### 3. OpenShift Deployment Commands

To apply the pipeline and execute it on an OpenShift cluster:

```bash
# 1. Create PersistentVolumeClaim
oc apply -f .tekton/pvc.yml

# 2. Apply Custom Tasks
oc apply -f .tekton/tasks.yml

# 3. Apply the Pipeline
oc apply -f .tekton/pipeline.yml

# 4. Trigger the PipelineRun
tkn pipeline start oc-pipelines-oc-final \
  --workspace name=pipeline-workspace,claimName=pipeline-pvc \
  --param git-url=https://github.com/Kushagra-Garg-27/ci-cd-final-project.git \
  --param git-revision=main \
  --showlog
```

---

## Verification Steps Summary

1. **Linting Check:** Run `flake8 .` -> Expect exit code 0.
2. **Unit Test Check:** Run `nosetests` -> Expect 9 tests OK and high coverage.
3. **App Check:** Run app and curl `/health` -> Expect `{"status":"OK"}`.
4. **GitHub Actions Check:** Check repository Actions tab -> Expect green checkmark on all steps.
5. **OpenShift Check:** Check `PipelineRuns` -> Status `Succeeded` (Green).

---

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.

## Author

Kushagra Garg (IBM Developer Skills Network / Coursera CI/CD Final Project)
