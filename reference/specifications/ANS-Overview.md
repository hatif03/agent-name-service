# Agent Name Service (ANS) - Specification Overview

## Purpose

The Agent Name Service (ANS) is a protocol-agnostic directory designed to facilitate secure discovery and interaction among AI agents. It provides a standardized way to register, discover, and verify AI agents across different platforms and protocols.

## Core Concepts

### Agent Identity

An agent in ANS is identified by:
- **Name**: Human-readable identifier (e.g., `weather-agent`)
- **Domain**: Hierarchical domain structure (e.g., `agents.example.com`)
- **Full Qualified Name (FQN)**: Combination of name and domain (e.g., `weather-agent.agents.example.com`)

### Agent Registration

Agents must be registered with:
- Valid PKI certificate
- Capability declarations
- Endpoint information
- Protocol support information
- Metadata (optional)

### Agent Resolution

Resolution can be:
- **Name-based**: Resolve by FQN
- **Capability-based**: Find agents with specific capabilities
- **Protocol-based**: Filter by supported protocols

## Naming Conventions

ANS follows DNS-inspired naming conventions:

```
<agent-name>.<subdomain>.<domain>.<tld>
```

Examples:
- `weather-agent.agents.example.com`
- `data-processor.services.mycompany.io`
- `chatbot.public.agents.net`

## Key Specifications

### Registration Schema

```json
{
  "name": "string",
  "domain": "string",
  "capabilities": ["string"],
  "endpoint": "string",
  "protocol": "A2A" | "MCP" | "ACP",
  "metadata": {
    "version": "string",
    "description": "string",
    "author": "string"
  },
  "certificate": "string (PEM format)"
}
```

### Resolution Response

```json
{
  "agentId": "string",
  "fqn": "string",
  "capabilities": ["string"],
  "endpoint": "string",
  "protocol": "string",
  "certificate": "string",
  "validUntil": "ISO 8601 timestamp",
  "metadata": {}
}
```

## Protocol Support

ANS supports multiple agent communication protocols:

1. **A2A (Agent-to-Agent)**: Direct agent communication protocol
2. **MCP (Model Context Protocol)**: Protocol for model context exchange
3. **ACP (Agent Communication Protocol)**: Standardized agent communication

## Security Requirements

- All agents must have valid PKI certificates
- Certificates must be issued by trusted CA
- Registration requires identity verification
- Threat analysis is performed on registration

## References

- OWASP GenAI Security Project - ANS v1.0 Specification
- DNS RFC 1035 (naming inspiration)
- X.509 PKI standards

