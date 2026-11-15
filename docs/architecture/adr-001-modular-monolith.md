# ADR-001: Modular Monolith Architecture

**Status:** Accepted  
**Date:** 2026-11-15

## 1. Context

CONTAINERHUB is a cross-platform container management platform.

The system requires multiple independent business modules, secure communication with Docker environments, user management, monitoring and resource management.

The architecture must support long-term development, maintainability, testability and future scalability.

## 2. Decision

The backend will use a Modular Monolith architecture based on Symfony 7.4 LTS.

Each business module will have four architectural layers:

- Domain
- Application
- Infrastructure
- Presentation

Modules must maintain explicit boundaries and avoid direct dependencies on the internal implementation of other modules.

Shared functionality will be placed in the Shared layer.

The Go-based System Agent will operate as a separate process and communicate with the backend through authenticated interfaces.

## 3. Alternatives Considered

### Microservices

Rejected for the initial development stage because they introduce additional infrastructure, deployment and operational complexity.

### Traditional Monolith

Rejected because it does not provide sufficiently explicit module boundaries for the planned system.

### Modular Monolith

Selected because it combines a single backend deployment model with clear internal separation of business modules.

## 4. Consequences

### Positive

- Clear separation of business responsibilities.
- Easier local development and deployment.
- Simplified debugging and testing.
- Reduced infrastructure complexity.
- Possibility of extracting individual modules into services in the future.

### Negative

- Module boundaries must be enforced through code review and automated tests.
- Shared database access requires careful architectural control.
- Incorrect dependencies can gradually weaken modularity.

## 5. Architectural Constraints

- Domain logic must not depend on Symfony.
- Modules must communicate through explicitly defined interfaces.
- Infrastructure implementations must remain separate from business logic.
- Cross-module dependencies must be intentional.
- The Go System Agent remains outside the Symfony application.
