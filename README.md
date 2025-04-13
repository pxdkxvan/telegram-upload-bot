# Telegram Upload Bot Project

This project is a Telegram bot designed to manage file uploads and downloads for authorized users. The bot allows users to upload files, receive secure download links, store links per user, and clear them when needed. Files are retrieved from Telegram's official storage using the Telegram API. The system is built as a collection of Spring Boot microservices, leveraging Spring Cloud Config for centralized configuration.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Bot Commands](#bot-commands)
- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [CI/CD Integration](#cicd-integration)
- [Security](#security)
- [Contributing](#contributing)
- [License](#license)

## Overview

The Telegram bot provides the following core functionality:
- **File Uploads**: Authorized users can upload files via the bot, which generates a download link.
- **Link Storage**: Stores download links for each user in a PostgreSQL database.
- **Link Management**: Allows users to view or clear their stored links.
- **Verification**: Supports user registration and email verification for secure access.
- **File Downloads**: Provides a private REST API for downloading files.

The project consists of five microservices, each handling specific responsibilities:
- **Config Service**: Centralizes configuration management using Spring Cloud Config, fetching settings from a private Git repository.
- **Migration Service**: Manages PostgreSQL database schema using Flyway migrations.
- **Dispatcher Service**: Receives Telegram messages, routes them to RabbitMQ queues based on type, and handles responses.
- **Node Service**: Processes commands from RabbitMQ queues using a Spring State Machine, interacts with the database and external APIs, and sends responses back.
- **Rest Service**: Exposes a private REST API for sending verification emails, processing verifications, and serving file downloads.

## Features

- **Secure File Handling**: Uploads files via Telegram and retrieves them using the Telegram API.
- **User Authentication**: Supports registration and email verification.
- **Command-Based Interaction**: Offers a rich set of commands for managing files and user sessions.
- **Distributed Architecture**: Uses microservices with RabbitMQ for messaging and PostgreSQL for persistence.
- **Monitoring**: Includes Spring Boot Actuator for health checks and metrics across all services.
- **Scalable Configuration**: Centralizes settings with Spring Cloud Config and profiles.

## Architecture

The system is composed of the following microservices:

1. **Config Service**:
    - Fetches configuration files `application-<profile>.yml` from a private Git repository.
    - Supports profiles: `actuator`, `openfeign`, `postgres`, `flyway`, `rabbitmq`, `telegram`, `smtp`.
    - Uses Spring Cloud Config Server.

2. **Migration Service**:
    - Creates and maintains database tables using Flyway.
    - Connects to PostgreSQL.
    - Supports profiles: `actuator`, `flyway`, `postgres`.

3. **Dispatcher Service**:
    - Receives messages from a Telegram bot.
    - Routes messages to RabbitMQ queues based on type.
    - Processes responses from other services.
    - Supports profiles: `telegram`, `rabbitmq`, `actuator`.

4. **Node Service**:
    - Consumes RabbitMQ messages from the Dispatcher.
    - Processes commands using a Spring State Machine (single-phase or two-phase).
    - Interacts with PostgreSQL and external APIs via OpenFeign.
    - Sends responses back via RabbitMQ.
    - Supports profiles: `telegram`, `rabbitmq`, `postgres`, `openfeign`, `actuator`.

5. **Rest Service**:
    - Provides a private REST API for:
        - Sending verification emails.
        - Processing user verification.
        - Downloading files.
    - Integrates with PostgreSQL and SMTP.
    - Supports profiles: `postgres`, `smtp`, `actuator`.

## Bot Commands

For a detailed list of bot commands and their usage, see [COMMANDS.md](COMMANDS.md).

## Prerequisites

To run the project, ensure you have:
- Java 21 or higher
- Spring Boot 3.4.x
- Gradle for dependency management
- Access to:
    - A private Git repository (for Config Service)
    - A PostgreSQL database
    - A RabbitMQ server
    - An SMTP server (e.g., Gmail)
    - A Telegram bot token
- Docker (optional, for containerized deployment)

## Setup

Follow these steps to set up and run the project:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/pxdkxvan/telegram-upload-bot.git
   ```
2. **Configure Environment Variables**:
    - Create a `.env` file in the project root (or each microservice directory, depending on your setup).
    - Populate it with the following variables (see details below):
      ```properties
      CONFIG_ENCRYPT_KEY=<encryption-key>

      GIT_CONFIG_SERVER=<git-repo-url>
      GIT_BRANCH=<git-branch>
      GIT_USER=<git-username>
      GIT_PASSWORD=<git-password>

      CONFIG_NAME=<config-service-name>
      MIGRATION_NAME=<migration-service-name>
      DISPATCHER_NAME=<dispatcher-service-name>
      NODE_NAME=<node-service-name>
      REST_NAME=<rest-service-name>
      
      CONFIG_HOST=<config-host>
      REST_HOST=<rest-host>

      CONFIG_PORT=<config-service-port>
      MIGRATION_PORT=<migration-service-port>
      DISPATCHER_PORT=<dispatcher-service-port>
      NODE_PORT=<node-service-port>
      REST_PORT=<rest-service-port>

      APPLICATION_PROTOCOL=<http-or-https>
      APPLICATION_HOST=<domain-or-ip>

      FLYWAY_LOCATIONS=<flyway-migration-path>

      OPENFEIGN_NAME=<feign-client-name>
      OPENFEIGN_HOST=<feign-client-host>
      OPENFEIGN_PORT=<feign-client-port>

      POSTGRES_HOST=<postgres-host>
      POSTGRES_PORT=<postgres-port>
      POSTGRES_USER=<postgres-user>
      POSTGRES_PASSWORD=<postgres-password>
      
      RABBITMQ_HOST=<rabbitmq-host>
      RABBITMQ_PORT=<rabbitmq-port>
      RABBITMQ_USER=<rabbitmq-user>
      RABBITMQ_PASSWORD=<rabbitmq-password>
      
      SMTP_PORT=<smtp-port>

      ACTUATOR_ENDPOINT=<actuator-base-path>
      ```
      See [ENVIRONMENT.md](ENVIRONMENT.md) for a detailed explanation of each `.env` variable.
3. **Build the Project**:
    - Navigate to each microservice directory and build:
      ```bash
      ./gradlew build
      ```
4. **Run the Services**:
    - Start each microservice in the following order to ensure dependencies are met:
        1. Config Service
        2. Migration Service
        3. Dispatcher Service
        4. Node Service
        5. Rest Service
    - Run each service with:
      ```bash
      ./gradlew bootRun
      ```
5. **Configure Telegram Bot**:
    - Ensure the Telegram bot token is set in the Dispatcher and Node services’ configurations (via Config Service).
    - Verify the bot is accessible via Telegram.

## Configuration

The project uses **Spring Cloud Config** for centralized configuration management. Configuration files are stored in a private Git repository and follow the naming convention `application-<profile>.yml`. Supported profiles include:
- `actuator`: Configures Actuator endpoints (`health`, `info`, `metrics`, `loggers`).
- `flyway`: Manages Flyway database migrations.
- `openfeign`: Sets up OpenFeign clients for external APIs.
- `postgres`: Configures PostgreSQL connections.
- `rabbitmq`: Configures RabbitMQ messaging.
- `smtp`: Configures SMTP for email delivery.
- `telegram`: Configures Telegram bot integration.

Each microservice imports the required profiles using `spring.config.import` from the Config Service.

## Deployment

For production deployment:

- Use a container orchestration tool like Docker or Kubernetes.
- Set up a secrets manager (e.g., AWS Secrets Manager, HashiCorp Vault) for sensitive variables.
- Ensure secure communication (HTTPS, SSL) for all services.
- Configure monitoring and logging for Actuator endpoints.

**Note**: Each microservice has its own `Dockerfile`, and a `docker-compose.yml` is provided for the entire project.

## CI/CD Integration

To integrate with a CI/CD pipeline:
- Build each microservice using Gradle.
- Run tests with `./gradlew test`.
- Deploy services in the order: Config, Migration, Dispatcher, Node, Rest.
- Use environment-specific `.env` files for different stages (dev, staging, prod).

## Security

- Encrypt sensitive data (e.g., database passwords, Telegram bot token) using `CONFIG_ENCRYPT_KEY`.
- Restrict access to the Rest Service’s API with authentication and authorization.
- Secure Actuator endpoints with Spring Security.
- Use HTTPS for Telegram API, RabbitMQ, and PostgreSQL connections.
- Regularly rotate credentials stored in the `.env` file.

## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss ideas, improvements, or bug fixes.

## License

This project is licensed under the [MIT License](LICENSE).