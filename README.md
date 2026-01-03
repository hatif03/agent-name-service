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

## Quick Start Example

Here's a complete example of setting up and using ANS:

```typescript
import { AgentNameService } from './src/ans';

async function main() {
  // Initialize ANS
  const ans = new AgentNameService({
    dbPath: './agent_registry.db',
    caName: 'ANS Root CA',
    enableThreatAnalysis: true
  });

  // Register a weather agent
  const weatherAgent = await ans.registerAgent({
    name: 'weather',
    domain: 'agents.example.com',
    capabilities: ['weather-forecast', 'location-based', 'real-time'],
    endpoint: 'https://api.example.com/weather',
    protocol: 'A2A',
    metadata: {
      version: '1.0.0',
      description: 'Provides weather forecasts for any location'
    }
  });

  console.log('Agent registered:', weatherAgent.agentId);

  // Resolve the agent
  const resolved = await ans.resolveAgent('weather.agents.example.com');
  if (resolved) {
    console.log('Resolved agent:', resolved.name);
    console.log('Capabilities:', resolved.capabilities);
    console.log('Certificate valid until:', resolved.certificate.validUntil);
  }

  // List all agents in a domain
  const agents = await ans.listAgents({ domain: 'agents.example.com' });
  console.log(`Found ${agents.length} agents in domain`);
}

main().catch(console.error);
```

## Development

### Building from Source

```bash
# Clone the repository
git clone <repository-url>
cd agent-name-service

# Install dependencies
npm install

# Build the project
npm run build

# Run in development mode
npm run dev
```

### Running Tests

```bash
# Run all tests
npm test

# Run tests with coverage
npm run test:coverage

# Run tests in watch mode
npm run test:watch
```

### Code Quality

```bash
# Lint the codebase
npm run lint

# Format code
npm run format

# Type checking
npm run type-check
```

## Deployment

### Docker Deployment

```bash
# Build Docker image
docker build -t agent-name-service .

# Run container
docker run -d \
  -p 8080:8080 \
  -v $(pwd)/data:/app/data \
  -e DB_PATH=/app/data/agent_registry.db \
  agent-name-service
```

### Production Considerations

- **Database**: Use a production-grade database (PostgreSQL, MongoDB) instead of SQLite for scalability
- **Certificate Storage**: Store CA certificates securely (use secrets management)
- **HTTPS**: Always use HTTPS in production environments
- **Rate Limiting**: Implement rate limiting for registration and resolution endpoints
- **Monitoring**: Set up logging and monitoring for agent health and registry operations
- **Backup**: Regular backups of the agent registry database

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DB_PATH` | Path to the registry database | `./agent_registry.db` |
| `CA_NAME` | Certificate Authority name | `ANS Root CA` |
| `CA_ORG` | CA organization name | `Agent Name Service` |
| `CERT_VALIDITY_DAYS` | Certificate validity period | `365` |
| `ENABLE_THREAT_ANALYSIS` | Enable threat analysis | `true` |
| `MASTRA_ENDPOINT` | Mastra threat analysis endpoint | `http://0.0.0.0:4111` |
| `REGISTRY_PORT` | Registry service port | `8080` |
| `REGISTRY_HOST` | Registry service host | `localhost` |

## Troubleshooting

### Common Issues

#### Certificate Validation Errors

**Problem**: Agents fail to register due to certificate validation errors.

**Solution**: 
- Ensure the CA certificate is properly initialized
- Check certificate expiration dates
- Verify certificate chain is complete

```typescript
// Check CA status
const caStatus = await ans.getCAStatus();
console.log('CA Status:', caStatus);
```

#### Agent Resolution Fails

**Problem**: Cannot resolve registered agents.

**Solution**:
- Verify agent name format (should be `name.domain`)
- Check if agent registration was successful
- Ensure agent hasn't expired

```typescript
// Debug resolution
const agent = await ans.resolveAgent('agent-name.domain.com');
if (!agent) {
  const allAgents = await ans.listAgents();
  console.log('Available agents:', allAgents.map(a => a.fqdn));
}
```

#### Database Lock Errors

**Problem**: SQLite database lock errors in concurrent scenarios.

**Solution**:
- Use connection pooling
- Switch to PostgreSQL or another production database
- Implement proper transaction handling

#### Threat Analysis Timeout

**Problem**: Threat analysis takes too long or times out.

**Solution**:
- Check Mastra endpoint connectivity
- Increase timeout values
- Disable threat analysis for development (`ENABLE_THREAT_ANALYSIS=false`)

## Performance & Scalability

### Optimization Tips

- **Caching**: Implement caching for frequently resolved agents
- **Database Indexing**: Ensure proper indexes on agent names and domains
- **Connection Pooling**: Use connection pooling for database connections
- **Load Balancing**: Distribute registry load across multiple instances
- **CDN**: Use CDN for certificate distribution

### Benchmarks

Typical performance characteristics:
- Agent registration: ~100-200ms (with threat analysis)
- Agent resolution: ~10-50ms
- Certificate generation: ~50-100ms
- Concurrent registrations: Supports 100+ concurrent operations

## FAQ

### What is the difference between ANS and DNS?

ANS is specifically designed for AI agents with:
- PKI-based identity verification
- Capability-aware resolution
- Protocol-agnostic support (A2A, MCP, ACP)
- Threat analysis integration
- Agent lifecycle management

### Can I use ANS with existing DNS infrastructure?

Yes, ANS can coexist with DNS. Agent names can follow DNS-like conventions but are resolved through the ANS registry.

### How do I handle certificate renewal?

Certificates can be renewed using the `renewRegistration()` method. Set up automated renewal before certificate expiration.

### Is ANS decentralized?

The current implementation uses a centralized registry. Future versions may support decentralized architectures using blockchain or distributed ledger technology.

### What protocols are supported?

Currently supports:
- **A2A** (Agent-to-Agent)
- **MCP** (Model Context Protocol)
- **ACP** (Agent Communication Protocol)

Additional protocols can be added via the protocol adapter layer.

### How secure is ANS?

ANS implements multiple security layers:
- PKI-based identity verification
- Threat analysis for registered agents
- Certificate validation
- Impersonation prevention through cryptographic verification

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Workflow

1. Read the [Contributing Guidelines](CONTRIBUTING.md)
2. Check existing issues and pull requests
3. Create an issue for major changes
4. Write tests for new features
5. Ensure all tests pass
6. Update documentation as needed
7. Submit pull request with clear description

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

