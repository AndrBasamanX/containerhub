# ADR-002: Go System Agent and Docker Integration

**Status:** Accepted  
**Date:** 2026-11-15

## 1. Context

CONTAINERHUB must manage Docker environments across Linux, Windows and macOS.

The Symfony backend must not directly access Docker Engine on remote hosts.

A separate system component is required to provide secure communication between the backend and Docker environments.

## 2. Decision

A standalone Go application will be developed as the CONTAINERHUB System Agent.

The agent will run on each managed host and communicate with the Symfony backend through an authenticated HTTPS interface protected by mutual TLS (mTLS).

The agent will interact with Docker Engine through its local API.

## 3. Agent Responsibilities

The System Agent will provide:

- Docker Engine connectivity.
- Container lifecycle management.
- Image management.
- Network management.
- Volume management.
- Container logs retrieval.
- Resource statistics collection.
- Host information reporting.
- Agent health monitoring.

## 4. Security Requirements

- All backend-agent communication must use mTLS.
- Each agent must have a unique cryptographic identity.
- Agent certificates must support rotation and revocation.
- The agent must expose only explicitly defined operations.
- Arbitrary shell command execution is prohibited.
- Docker Socket must never be exposed publicly.
- All privileged operations must be auditable.
- Agent access to Docker Engine must be treated as highly privileged.
- Agent must authorize every operation based on the authenticated backend identity and predefined permissions.
- The agent must reject unsupported operations and invalid request parameters.
- Every privileged operation must include an auditable request identifier.

## 5. Communication Protocol

The initial implementation will use HTTPS with JSON-based request and response messages.

Operations must use predefined request and response structures.

The agent must validate incoming requests before interacting with Docker Engine.

The backend must authenticate the agent and associate it with a registered host.

## 6. Alternatives Considered

### Direct Docker API Access from Symfony

Rejected because it tightly couples the backend to Docker hosts and increases the security exposure of the web application.

### Go System Agent

Selected because Go provides a suitable runtime for standalone cross-platform system applications.

### SSH-Based Management

Not selected as the primary communication mechanism because it introduces SSH credential management and remote command execution concerns.

## 7. Consequences

### Positive

- Separation between business logic and system-level operations.
- Centralized management of Docker environments.
- Cross-platform agent implementation.
- Explicit security boundary.
- Independent agent deployment and updates.

### Negative

- Additional software component to maintain.
- Certificate lifecycle management is required.
- Agent availability affects host management operations.
- Docker access requires careful privilege management.

## 8. Architectural Constraints

- The agent must remain independent of Symfony.
- The agent must not execute arbitrary shell commands.
- Only explicitly supported Docker operations are allowed.
- All communication must be authenticated.
- Docker Engine access must remain local to the managed host.
