# Health Check Verification Report

## Objective

Verify that the Orders API correctly detects application
health and database availability using Docker health checks.

## 1. Healthy State

The application and PostgreSQL database were started
using Docker Compose.

Observed:
- orders-api reported healthy.
- db reported healthy.
- GET /health returned HTTP 200.
- GET /ready returned HTTP 200.

Evidence: `evidence/healthy.txt`

## 2. Database Failure

The PostgreSQL container was stopped using:

`docker compose stop db`

Observed:
- The API process remained running.
- GET /health continued returning HTTP 200.
- GET /ready returned HTTP 503.
- Docker marked orders-api unhealthy.

Evidence: `evidence/unhealthy.txt`

## 3. Database Recovery

PostgreSQL was restarted using:

`docker compose start db`

Observed:
- PostgreSQL became available again.
- GET /ready returned HTTP 200.
- Docker marked orders-api healthy again.

Evidence: `evidence/recovered.txt`

## 4. Container Logs

Evidence: `evidence/container-logs.txt`

## 5. Recovery Behaviour

The Docker Compose restart policy is set to
`unless-stopped`.

This policy restarts containers when their main process exits.
It does not automatically restart containers solely because
they become unhealthy.

The recovery test demonstrates that the application
automatically returns to a healthy state when its database
dependency is restored.
