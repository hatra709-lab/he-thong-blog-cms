# Blog CMS - Deployment and Monitoring

## 1. Introduction

This project deploys a Blog/CMS using WordPress and MySQL.
It includes phpMyAdmin for database management, Nginx as a
reverse proxy, Prometheus and Grafana for metrics monitoring,
and Loki with Grafana Alloy for centralized logging.

## 2. Architecture

- WordPress: Blog/CMS application
- MySQL 8.4: application database
- phpMyAdmin: database administration
- Nginx: reverse proxy, HTTPS and security headers
- Prometheus: metrics collection
- Grafana: dashboards and log exploration
- cAdvisor: container metrics
- Nginx Prometheus Exporter: Nginx metrics
- Loki: centralized log storage
- Grafana Alloy: Docker log collection and forwarding

## 3. Requirements

- Linux / Ubuntu
- Docker Engine
- Docker Compose
- OpenSSL

## 4. Environment configuration

Create a `.env` file containing the following variables:

- MYSQL_ROOT_PASSWORD
- MYSQL_DATABASE
- MYSQL_USER
- MYSQL_PASSWORD
- GRAFANA_ADMIN_PASSWORD

Use strong passwords and do not commit `.env` to GitHub.

## 5. HTTPS certificate

The lab uses a self-signed certificate.
Generate a certificate for the actual Ubuntu VM IP address
and update `nginx/default.conf` if the IP address changes.

## 6. Start the system

Validate the configuration:

    docker compose config -q

Start all services:

    docker compose up -d

Check service status:

    docker compose ps

Check service logs:

    docker compose logs --tail=50

## 7. Service endpoints

- Blog/CMS: https://VM_IP_ADDRESS/
- phpMyAdmin: http://VM_IP_ADDRESS:8081
- Prometheus: http://VM_IP_ADDRESS:9090
- Grafana: http://VM_IP_ADDRESS:3001

Replace VM_IP_ADDRESS with the actual IP address of the Ubuntu VM.
The HTTPS certificate is self-signed and may trigger a browser warning.

## 8. Monitoring

Prometheus collects metrics from Prometheus, cAdvisor and
the Nginx Prometheus Exporter.

Grafana uses Prometheus as a metrics data source. Loki is
used as a log data source, and Grafana Alloy collects Docker logs.

Example LogQL query:

    {container="blog-cms-nginx-1"}

Find HTTP 5xx responses when access logs contain the status code:

    {container="blog-cms-nginx-1"} |~ "\" 5[0-9]{2} "

## 9. Security

- Store credentials in `.env`.
- Do not commit `.env` or private keys.
- Keep MySQL on the internal Docker network.
- Use Nginx HTTPS and security headers.
- Verify exposed ports before deployment.
- Restrict access to administrative and monitoring interfaces.

## 10. Stop the system

    docker compose down

This command stops and removes containers and the project network.
It does not remove named volumes.

Do not use `docker compose down -v` unless you intentionally
want to delete the project's persisted data.
