---
icon: circle-nodes
---

# Nodes

### Overview

Nodes are fundamental components of the UOMI ecosystem, serving multiple critical functions including blockchain maintenance, AI computation execution, and system-wide service integration. Each node type is optimized for specific roles within the network architecture.

<figure><img src="../../.gitbook/assets/file (3).png" alt=""><figcaption></figcaption></figure>

### Node Types

#### Full Node

* **Core Functions:**
  * Maintains complete blockchain copy
  * Performs comprehensive transaction verification
  * Facilitates network transaction propagation
  * Supports network stability and decentralization

#### Archive Node

* **Key Features:**
  * Stores complete historical state
  * Enables historical block queries
  * Supports data analytics and explorers
  * Maintains network transparency

#### Validator Node

* **Capabilities:**
  * Operates full node functionality
  * Participates in consensus
  * Implements intelligent state pruning
  * Optimizes storage efficiency
  * Executes Agents

### MainNet Service Architecture

#### Core Components

1. **Uomi-node Service**
   * Substrate-based blockchain runtime
   * Transaction processing engine
   * State management system
   * Network communication protocol
2. **AI Service**
   * Dedicated computation engine
   * Model execution environment
   * Agent interaction interface
   * Resource management system

#### Integration Layer

The AI-AGENT system interfaces with node services through:

* Secure proxy functions
* Protected communication channels
* Resource allocation controls

### Technical Specifications

#### Hardware Requirements

| Node Type | CPU      | RAM   | Storage | Network | GPU           |
| --------- | -------- | ----- | ------- | ------- | ------------- |
| Full      | 4+ cores | 8GB+  | 100GB+  | 1 Gbps  | -             |
| Archive   | 8+ cores | 16GB+ | 500GB+  | 1 Gbps  | -             |
| Validator | 8+ cores | 16GB+ | 100GB+  | 1 Gbps  | 2 x RTX 4090+ |

#### Network Parameters

* Default P2P port: 30333
* Default RPC port: 9944
* Default WS port: 9944
* Default Prometheus port: 9615

### Security Considerations

1. **Network Security**
   * Firewall configuration
   * Port access control
   * DDoS protection
   * Secure communication protocols
2. **System Security**
   * Regular updates
   * Access control
   * Resource isolation
   * Monitoring and alerting
3. **Data Security**
   * State encryption
   * Secure key management
   * Backup procedures
   * Recovery protocols

### Best Practices

1. **Deployment**
   * Use dedicated hardware
   * Implement monitoring
   * Regular maintenance
   * Performance optimization
2. **Operation**
   * Regular backups
   * Update management
   * Resource monitoring
   * Performance tuning
3. **Maintenance**
   * Regular updates
   * Security patches
   * Performance monitoring
   * Capacity planning

This comprehensive guide provides the foundation for understanding and implementing UOMI network nodes. For specific setup instructions, refer to our node deployment documentation.
