# Environment Variables

The project relies on a `.env` file to configure sensitive and environment-specific settings. Below is a detailed explanation of each variable required in the `.env` file:

- **CONFIG_ENCRYPT_KEY**: Encryption key used to secure sensitive configuration data, such as database passwords, SMTP credentials, and the Telegram bot token.
- **GIT_CONFIG_SERVER**: URL of the private Git repository hosting configuration files for the Config Service (e.g., `https://github.com/your-repo/config`).
- **GIT_BRANCH**: Git branch to fetch configurations from (e.g., `main` or `prod`).
- **GIT_USER**: Username for authenticating with the Git repository.
- **GIT_PASSWORD**: Password for authenticating with the Git repository.
- **CONFIG_NAME**: Unique identifier for the Config Service, used for service discovery or logging (e.g., `config-service`).
- **MIGRATION_NAME**: Unique identifier for the Migration Service (e.g., `migration-service`).
- **DISPATCHER_NAME**: Unique identifier for the Dispatcher Service (e.g., `dispatcher-service`).
- **NODE_NAME**: Unique identifier for the Node Service (e.g., `node-service`).
- **REST_NAME**: Unique identifier for the Rest Service (e.g., `rest-service`).
- **CONFIG_HOST**: Domain or IP address where the Config Service is hosted, enabling network routing and service discovery (e.g., `config.example.com` or an IP address).
- **REST_HOST**: Domain or IP address where the Rest Service is hosted, facilitating external and internal access (e.g., `rest.example.com` or an IP address).
- **CONFIG_PORT**: Port on which the Config Service runs (e.g., `8888`).
- **MIGRATION_PORT**: Port for the Migration Service (e.g., `8081`).
- **DISPATCHER_PORT**: Port for the Dispatcher Service (e.g., `8082`).
- **NODE_PORT**: Port for the Node Service (e.g., `8083`).
- **REST_PORT**: Port for the Rest Service (e.g., `8084`).
- **APPLICATION_PROTOCOL**: Protocol for service communication, either `http` or `https`.
- **APPLICATION_HOST**: Domain or IP address for services (e.g., `localhost` for development or a production domain like `example.com`).
- **FLYWAY_LOCATIONS**: Path to Flyway migration scripts, typically a classpath location (e.g., `classpath:db/migration`).
- **OPENFEIGN_NAME**: Unique name for the OpenFeign client used in the Node Service (e.g., `external-api-client`).
- **OPENFEIGN_HOST**: Domain or IP address of the target service for the OpenFeign client, facilitating service-to-service communication (e.g., `api.example.com`).
- **OPENFEIGN_PORT**: Port for the OpenFeign client’s target service (e.g., `8080`).
- **POSTGRES_HOST**: Domain or IP address where the PostgreSQL database is hosted (e.g., `db.example.com` or `localhost`).
- **POSTGRES_PORT**: Port for the PostgreSQL database (e.g., `5432`).
- **POSTGRES_USER**: Username for the PostgreSQL database connection (e.g., `postgres`).
- **POSTGRES_PASSWORD**: Password for the PostgreSQL database connection (e.g., `mysecretpassword`).
- **RABBITMQ_HOST**: Domain or IP address where the RabbitMQ server is located, used for message brokering between services (e.g., `rabbitmq.example.com` or `localhost`).
- **RABBITMQ_PORT**: Port for the RabbitMQ server (e.g., `5672`).
- **RABBITMQ_USER**: Username for the RabbitMQ connection (e.g., `guest`).
- **RABBITMQ_PASSWORD**: Password for the RabbitMQ connection (e.g., `guest`).
- **SMTP_PORT**: Port for the SMTP server (e.g., `587` for Gmail).
- **ACTUATOR_ENDPOINT**: Base path for Spring Boot Actuator endpoints across all services (e.g., `/actuator`).

**Note**: Place the `.env` file in the project root or each microservice directory, depending on your setup. Sensitive variables (e.g., `GIT_PASSWORD`, `CONFIG_ENCRYPT_KEY`, `POSTGRES_PASSWORD`, `RABBITMQ_PASSWORD`) should be managed securely, ideally through a secrets manager in production.