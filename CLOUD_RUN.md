# Cloud Health Check Mapping

## Overview

The local Orders API implements two health endpoints:

- GET /health: Liveness check. Returns HTTP 200 when the
  application process is running.
- GET /ready: Readiness check. Checks PostgreSQL connectivity
  using SELECT 1. Returns HTTP 200 when the database is reachable
  and HTTP 503 when it is unavailable.

## Google Cloud Run Mapping

| Local Docker implementation | Cloud Run equivalent |
|---|---|
| /health | HTTP liveness probe |
| /ready | HTTP readiness probe |
| Application startup | Startup probe |
| Docker healthcheck | Cloud Run health probe configuration |

### Suggested Cloud Run configuration

- Startup probe: HTTP GET /health
- Liveness probe: HTTP GET /health
- Readiness probe: HTTP GET /ready

The startup probe allows the application time to initialize.
The liveness probe detects an unresponsive application process.
The readiness probe checks whether the application can access
its required database dependency.

## Kubernetes Mapping

| Local endpoint | Kubernetes probe |
|---|---|
| /health | Liveness probe |
| /ready | Readiness probe |
| Startup | Startup probe |

## Recovery Behaviour

Docker Compose restart: unless-stopped restarts a container
when its main process exits. It does not automatically restart
a container merely because its health status becomes unhealthy.

Cloud Run and Kubernetes provide their own health-management
and recovery mechanisms.

This document describes the architectural mapping only.
No cloud deployment is performed as part of this assignment.
