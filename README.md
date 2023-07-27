# Fetch
Sure! Below is a sample README file for your GitHub repository for the Fetch Rewards Data Engineering Take Home project:

# Fetch Rewards Data Engineering Take Home

This project demonstrates an ETL (Extract, Transform, Load) process for reading JSON data from an AWS SQS Queue, masking sensitive data, and writing the processed data to a PostgreSQL database. The application is designed to be run locally using Docker containers with pre-loaded data. The objective is to hide personally identifiable information (PII) in a way that duplicates can still be identified by data analysts.

## Project Overview

The project consists of a Python application that performs the following tasks:
1. Reads JSON data from an AWS SQS Queue.
2. Masks the PII fields (`device_id` and `ip`) using SHA256 hashing.
3. Writes the processed data to a PostgreSQL database.

## Getting Started

### Prerequisites

Make sure you have the following software installed on your local machine:
- Docker (https://docs.docker.com/get-docker/)
- Docker Compose
- `awscli-local` Python package
- PostgreSQL client (psql)

### Running the Application

To set up and run the application, follow these steps:

1. Clone the repository to your local machine:

```bash
git clone https://github.com/your_username/fetch-rewards-data-engineering.git
cd fetch-rewards-data-engineering
```

2. Use Docker Compose to run the test environment:

```bash
docker-compose up
```

3. Verify the Localstack and Postgres containers are running.

4. Read a message from the queue using awslocal:

```bash
awslocal sqs receive-message --queue-url http://localhost:4566/000000000000/login-queue
```

5. Connect to the Postgres database and verify the `user_logins` table is created:

```bash
psql -d postgres -U postgres -p 5432 -h localhost -W
postgres=# select * from user_logins;
```

### Application Structure

The project's main files and directories are structured as follows:

```
fetch-rewards-data-engineering/
│
├── app.py                # Main Python application
├── docker-compose.yml    # Docker Compose configuration for Localstack and Postgres
├── .env.example          # Example environment variables file (should be renamed to .env)
└── README.md             # Project documentation (you are here)
```

### Decisions and Assumptions

1. **Reading Messages from the Queue**:
   We use the `boto3` library in Python to interact with AWS and read messages from the SQS queue.

2. **Data Structures**:
   Python dictionaries are used to represent the JSON data read from the SQS queue.

3. **Masking PII Data**:
   PII data (`device_id` and `ip`) is masked using SHA256 hashing, ensuring that duplicates can still be identified.

4. **Connecting and Writing to Postgres**:
   The `psycopg2` library is used to connect to the PostgreSQL database and perform data insertion.

5. **Deployment in Production**:
   In a production environment, the application can be containerized using Docker and deployed using Kubernetes or Docker Swarm for better scalability and management.

### Next Steps

If we had more time, we would implement the following to make the application production-ready:
- Implement error handling and logging for better reliability and monitoring.
- Write unit tests to ensure correctness and robustness.
- Use configuration management to handle credentials and connection settings.
- Deploy the application using container orchestration for scaling and high availability.
- Set up monitoring and alerting to detect and respond to issues proactively.

