# CONTAINERHUB — Architecture Overview

## 1. Project Overview

CONTAINERHUB is a cross-platform container management platform designed to manage Docker environments through a secure web interface.

Supported operating systems:
- Linux
- Windows
- macOS

## 2. Technology Stack

| Component | Technology |
|---|---|
| Backend | PHP 8.3 |
| Framework | Symfony 7.4 LTS |
| Frontend | React + TypeScript |
| Database | PostgreSQL |
| Cache and Messaging | Redis |
| System Agent | Go |
| Container Runtime | Docker Engine |
| Web Server | Nginx |
| API | REST API |

## 3. Architecture

The backend follows a modular monolith architecture.

Each business module is divided into four layers:

- Domain — business entities, value objects and domain rules.
- Application — use cases, commands, queries and application services.
- Infrastructure — database access, external integrations and technical implementations.
- Presentation — HTTP controllers and API endpoints.

## 4. Core Modules

- Dashboard
- Host Manager
- Container Manager
- Image Manager
- Network Manager
- Volume Manager
- Logs Manager
- Resource Monitor
- Compose Manager
- Registry Manager
- User & Access Manager
- Audit Manager
- Settings & Automation

## 5. Security Principles

- Role-Based Access Control (RBAC).
- Mutual TLS (mTLS) between backend and system agent.
- Audit logging for sensitive operations.
- Least-privilege principle.
- No public exposure of Docker socket.
- No arbitrary host shell command execution.

## 6. Communication

The frontend communicates with the backend through REST API.

The backend communicates with the Go agent through authenticated, explicitly defined operations.

The agent interacts with Docker Engine through its local API.

## 7. Architectural Principles

- Separation of concerns.
- Dependency inversion.
- Explicit module boundaries.
- Domain logic independent of Symfony.
- Secure-by-default design.
- Automated testing.
