# Agent Name Service (ANS)

A protocol-agnostic directory service designed to facilitate secure discovery and interaction among AI agents. The Agent Name Service maps human-readable agent identifiers to verifiable metadata, endpoints, and capabilities, enabling robust, scalable, and interoperable multi-agent ecosystems.

## Overview

The Agent Name Service (ANS) is inspired by DNS (Domain Name System) but specifically designed for AI agents. It provides a secure, decentralized way to discover, register, and interact with AI agents across different protocols and platforms.

## Key Features

### 🔐 Secure Agent Discovery
- **PKI-Based Identity**: Utilizes Public Key Infrastructure (PKI) certificates to ensure verifiable agent identities and trustworthiness
- **Threat Analysis**: Comprehensive security assessment and threat modeling for registered agents
- **Certificate Authority**: Built-in CA/RA (Certificate Authority/Registration Authority) for identity validation

### 🌐 Protocol-Agnostic Registry
- **Multi-Protocol Support**: Supports diverse communication standards including:
  - **A2A** (Agent-to-Agent)
  - **MCP** (Model Context Protocol)
  - **ACP** (Agent Communication Protocol)
- **Protocol Adapter Layer**: Modular architecture that decouples registry implementation from communication standards

### 📝 Structured Naming Conventions
- **DNS-Inspired Naming**: Familiar naming conventions similar to DNS
- **Capability-Aware Resolution**: Dynamic resolution based on agent capabilities
- **Hierarchical Structure**: Supports hierarchical agent naming (e.g., `weather.agents.example.com`)

### 🔄 Lifecycle Management
- **Agent Registration**: Formalized registration process with metadata validation
- **Renewal Mechanisms**: Automated certificate and registration renewal
- **Status Tracking**: Monitor agent availability and health

## Use Cases

### 1. Multi-Agent Systems (MAS)
Enable secure discovery and communication between multiple AI agents working together on complex tasks.

### 2. AI Marketplaces
Facilitate secure and efficient discovery of AI services and agents in marketplace environments.

### 3. Autonomous Networks
Support dynamic, context-aware communication among autonomous agents in distributed systems.

### 4. Internet of Things (IoT)
Enable adaptive IoT applications through secure agent discovery and interaction.

### 5. Enterprise AI Integration
Provide a centralized registry for enterprise AI agents, ensuring secure and standardized communication.

### 6. Agent Orchestration
Enable orchestration platforms to discover and coordinate multiple specialized agents.

## Architecture

The ANS architecture comprises several key components:

```
┌─────────────────────────────────────────────────────────┐
│                    Agent Name Service                    │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│  │   Registry   │  │  Certificate │  │   Protocol   │ │
│  │   Database   │  │  Authority   │  │   Adapters   │ │
│  └──────────────┘  └──────────────┘  └──────────────┘ │
│                                                           │
│  ┌──────────────────────────────────────────────────┐   │
│  │         Threat Analysis & Security Layer          │   │
│  └──────────────────────────────────────────────────┘   │
│                                                           │
└─────────────────────────────────────────────────────────┘
```

### Core Components

1. **Agent Registry**: Distributed database storing agent profiles, capabilities, and PKI credentials
2. **Certificate Authority (CA)**: Validates identities and issues certificates
3. **Protocol Adapter Layer**: Enables multi-protocol registration and lookup
4. **Threat Analysis Engine**: Performs security assessments on registered agents
5. **Resolution Service**: Handles agent name resolution and capability matching

## Getting Started

### Prerequisites

- Node.js (v18 or higher)
- npm or pnpm package manager
- Basic understanding of PKI and certificate management

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd agent-name-service

# Install dependencies
npm install
# or
pnpm install
```

### Configuration

Create a `.env` file in the root directory:

```env
# Database Configuration
DB_PATH=./agent_registry.db

# Certificate Authority Configuration
CA_NAME="ANS Root CA"
CA_ORG="Agent Name Service"
CERT_VALIDITY_DAYS=365

# Security Configuration
ENABLE_THREAT_ANALYSIS=true
MASTRA_ENDPOINT=http://0.0.0.0:4111

# Registry Configuration
REGISTRY_PORT=8080
REGISTRY_HOST=localhost
```

### Basic Usage

```typescript
import { AgentNameService } from './src/ans';

// Initialize the ANS service
const ans = new AgentNameService({
  dbPath: './agent_registry.db',
  caName: 'ANS Root CA',
  enableThreatAnalysis: true
});

// Register an agent
const registration = await ans.registerAgent({
  name: 'weather-agent',
  domain: 'agents.example.com',
  capabilities: ['weather-forecast', 'location-based'],
  endpoint: 'https://api.example.com/weather',
  protocol: 'A2A'
});

// Resolve an agent
const agent = await ans.resolveAgent('weather-agent.agents.example.com');
console.log(agent);
```

## Project Structure

```
agent-name-service/
├── src/                      # Source files
│   ├── ans.ts                # Core ANS service
│   ├── certificate.ts        # Certificate generation and validation
│   ├── db.ts                 # Database layer for agent registry
│   ├── protocols.ts         # Protocol formatters (A2A, MCP, ACP)
│   └── types/                # TypeScript type definitions
├── tests/                    # Test files
├── examples/                 # Usage examples
├── docs/                     # Documentation
├── reference/                # Reference materials and research
│   ├── specifications/       # Protocol specifications
│   ├── security/             # Security best practices
│   └── use-cases/            # Detailed use case examples
└── README.md                 # This file
```

## API Reference

### Register Agent

Register a new agent with the ANS.

```typescript
registerAgent(params: {
  name: string;
  domain: string;
  capabilities: string[];
  endpoint: string;
  protocol: 'A2A' | 'MCP' | 'ACP';
  metadata?: Record<string, any>;
}): Promise<RegistrationResult>
```

### Resolve Agent

Resolve an agent by name and optionally filter by capabilities.

```typescript
resolveAgent(
  name: string,
  capabilities?: string[]
): Promise<Agent | null>
```

### List Agents

List all registered agents, optionally filtered by domain or capabilities.

```typescript
listAgents(filters?: {
  domain?: string;
  capabilities?: string[];
}): Promise<Agent[]>
```

### Renew Registration

Renew an agent's registration and certificate.

```typescript
renewRegistration(
  agentId: string
): Promise<RegistrationResult>
```

## Security Considerations

- **Certificate Validation**: All agents must have valid PKI certificates
- **Threat Analysis**: Registered agents undergo security assessment
- **Impersonation Prevention**: Cryptographic verification prevents agent impersonation
- **Registry Poisoning Mitigation**: Multiple validation layers prevent malicious registrations

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Resources

- [OWASP GenAI Security Project - ANS Specification](https://genai.owasp.org/resource/agent-name-service-ans-for-secure-al-agent-discovery-v1-0/)
- [Reference Materials](./reference/) - Detailed specifications and use cases
- [Examples](./examples/) - Code examples and tutorials

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- OWASP GenAI Security Project for the ANS specification
- The open-source community for inspiration and feedback

---

**Note**: This is an active research and development project. Features and APIs may change as the project evolves.

