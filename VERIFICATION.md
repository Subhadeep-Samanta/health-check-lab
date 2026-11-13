# Health Check Verification

## 1. Healthy State

Expected observations:

- orders-api reports healthy.
- db reports healthy.
- GET /health returns HTTP 200.
- GET /ready returns HTTP 200.

Evidence: evidence/healthy.txt

## 2. Database Failure

Test:

docker compose stop db

Expected observations:

- Application process remains alive.
- GET /health returns HTTP 200.
- GET /ready returns HTTP 503.
- orders-api becomes unhealthy.

Evidence: evidence/unhealthy.txt

## 3. Database Recovery

Test:

docker compose start db

Expected observations:

- PostgreSQL becomes available.
- GET /ready returns HTTP 200.
- orders-api returns to healthy.

Evidence: evidence/recovered.txt

## 4. Container Logs

Evidence: evidence/container-logs.txt

## 5. Recovery Limitation

Docker Compose restart: unless-stopped does not restart a
container solely because its health status becomes unhealthy.
The recovery demonstrated here is restoration of readiness
after the database dependency is restored.
