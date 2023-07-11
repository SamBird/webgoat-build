# Tier 1 Bank Web Application

This repository contains the source code for the Tier 1 Bank web application.

## Jenkins Pipeline Stages

The Jenkinsfile in this repository defines a CI/CD pipeline with the following stages:

1. **Builds with Maven:** The pipeline starts by building the application using Maven. It compiles the source code, resolves dependencies, and packages the application artifacts.

2. **Tests with Maven:** After the build stage, the pipeline runs tests using Maven. It executes unit tests and any other configured test suites to ensure the application's functionality.

3. **Runs SonarScan:** Following the testing stage, the pipeline performs a SonarScan. This stage analyzes the code for quality, security, and maintainability using SonarQube. It provides detailed reports and metrics to assess code health.

4. **Builds Docker Image:** Once the code passes the SonarScan stage, the pipeline builds a Docker image of the application. It uses a Dockerfile to define the image configuration and includes the necessary dependencies and artifacts.

5. **Runs Helm Lint and Package:** Finally, the pipeline verifies the Helm chart for the application. It performs a Helm lint check to ensure the chart follows best practices and validates its structure. Then, it packages the Helm chart for deployment.
