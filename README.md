# CI/CD Final Project - Hit Counter Microservice

## Project Overview
This repository contains the completed final project for the **Continuous Integration and Continuous Delivery (CI/CD)** course, part of the **IBM DevOps and Software Engineering Professional Certificate**.

The project implements a complete, automated DevOps CI/CD pipeline for a RESTful Hit Counter microservice:
1. **Continuous Integration (CI)** with **GitHub Actions**: Automated static analysis, linting, code quality checks, and unit test execution on pull requests and pushes to `main`.
2. **Continuous Delivery (CD)** with **Tekton / OpenShift Pipelines**: Automated workspace cleanup, source checkout, linting, testing, container image building with Buildah, and automated deployment to Red Hat OpenShift.

---

## Author
- **Developer / Student**: `securecycle5`
- **Course**: IBM DevOps & Software Engineering - CI/CD

---

## Technologies Used
- **Application**: Python 3.9, Flask RESTful microservice, Gunicorn WSGI server
- **Testing & Quality Assurance**: Nose (`nosetests`), Flake8 (PEP 8 linter), Coverage
- **Continuous Integration**: GitHub Actions Workflows (`.github/workflows/workflow.yml`)
- **Continuous Deployment**: OpenShift Pipelines, Tekton Tasks & Pipelines (`.tekton/tasks.yml`, `.tekton/pipeline.yml`)
- **Containerization & Deployment**: Dockerfile, Buildah, Red Hat OpenShift Container Platform (OCP)

---

## Project Structure
```text
├── .github/
│   └── workflows/
│       └── workflow.yml        # GitHub Actions CI workflow (lint & nose tests)
├── .tekton/
│   ├── tasks.yml               # Custom Tekton tasks (cleanup, flake8, nose)
│   └── pipeline.yml            # Tekton pipeline definition (cleanup -> git-clone -> flake8 -> nose -> buildah -> deploy)
├── service/                    # Microservice Flask application source code
│   ├── __init__.py
│   ├── routes.py
│   └── models.py
├── tests/                      # Unit test suites executed by nosetests
│   ├── __init__.py
│   └── test_routes.py
├── Dockerfile                  # Container definition for the microservice
├── requirements.txt            # Python dependencies
└── README.md                   # Project documentation and details
```

---

## Pipeline Execution
- **GitHub Actions**: Triggers on every push/PR, installs requirements, executes `flake8`, and runs unit tests with `nosetests`.
- **OpenShift Pipelines**: Orchestrates 6 stages:
  1. `cleanup` - Wipes previous workspace contents.
  2. `git-clone` - Clones repository source code into persistent volume workspace.
  3. `flake8` - Enforces PEP 8 compliance and code style.
  4. `nose` - Runs unit tests and verifies code coverage.
  5. `buildah` - Builds the container image and pushes it to OpenShift internal registry.
  6. `deploy` - Applies Kubernetes deployment and service manifests to OpenShift.
