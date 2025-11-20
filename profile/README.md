<div align="center">

# Evalys

**The Privacy Layer for Memecoin Launchpads**

*Advanced privacy-preserving infrastructure for Solana-based memecoin trading*

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Solana](https://img.shields.io/badge/Solana-14C294?logo=solana&logoColor=white)](https://solana.com/)
[![Arcium](https://img.shields.io/badge/Arcium-Encrypted%20Compute-8B5CF6)](https://arcium.com/)

</div>

---

## Overview

**Evalys** is a modular, privacy-preserving infrastructure system designed specifically for memecoin launchpad platforms on Solana. Built with a focus on privacy, security, and composability, Evalys provides a complete toolkit for executing transactions while maintaining user anonymity and protecting against MEV attacks.

The system is architected as a collection of independent, reusable components that can operate standalone or integrate seamlessly to provide comprehensive privacy-preserving transaction execution capabilities.

**Evalys now integrates with Arcium's encrypted supercomputer** – enabling confidential computation for strategy planning, risk assessment, and curve analytics. Your trading intent, risk profile, and strategy are computed confidentially via MPC, while Evalys executes the resulting plan using burner swarms, MEV-safe routing, and launchpad adapters on Solana.

---

## Technical Architecture

Evalys follows a **layered architecture** where each layer provides specific functionality while maintaining clear separation of concerns. Components are designed to be modular, testable, and independently deployable.

### Layered System View

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           PRESENTATION LAYER                                 │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────┐   │
│  │                    evalys-web-ui                                      │   │
│  │  React + Redux Dashboard | Real-time Analytics | User Interface      │   │
│  └────────────────────────────────────────────────────────────────────┘   │
└────────────────────────────────────┬───────────────────────────────────────┘
                                      │
                                      │ HTTP/WebSocket
                                      │
┌─────────────────────────────────────▼───────────────────────────────────────┐
│                           APPLICATION LAYER                                   │
│                                                                               │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐            │
│  │ Privacy Engine   │  │ Curve Intel      │  │ Execution Engine │            │
│  │ API (Port 8001)  │  │ API (Port 8003) │  │ API (Port 8004)  │            │
│  └──────────────────┘  └──────────────────┘  └──────────────────┘            │
│                                                                               │
  │  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐            │
  │  │ Burner Swarm     │  │ Launchpad        │  │ Arcium Bridge   │            │
  │  │ API (Port 8002)  │  │ Adapters         │  │ API (Port 8010) │            │
  │  └──────────────────┘  │ API (Port 8005)  │  └──────────────────┘            │
  │                        └──────────────────┘  ┌──────────────────┐            │
  │                                               │ Arcium gMCP      │            │
  │                                               │ API (Port 8011)  │            │
  │                                               └──────────────────┘            │
└───────────────────────────────┬──────────────────────────────────────────────┘
                                │
                                │ Internal API Calls
                                │
┌───────────────────────────────▼──────────────────────────────────────────────┐
│                           CORE SERVICES LAYER                                  │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │                    Privacy Gradient Engine (PGE)                     │      │
│  │  • Mode Selection (Normal/Stealth/Max Ghost)                        │      │
│  │  • Dynamic Privacy Configuration                                    │      │
│  │  • Risk-Based Mode Adjustment                                       │      │
│  └────────────────────────────────────────────────────────────────────┘      │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │                    Burner Swarm Fabric                                │      │
│  │  • Wallet Pool Management (Active/Reserve/Retired)                   │      │
│  │  • Just-In-Time (JIT) Funding                                       │      │
│  │  • Wallet Rotation Strategies                                        │      │
│  │  • Encrypted Key Storage                                             │      │
│  └────────────────────────────────────────────────────────────────────┘      │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │                    Curve Intelligence Layer                          │      │
│  │  • Real-time Bonding Curve Analysis                                 │      │
│  │  • Risk Detection (Snipers, Clusters, Liquidity)                    │      │
│  │  • Execution Window Optimization                                    │      │
│  │  • Pattern Recognition (Whales, Bots, Anomalies)                     │      │
│  └────────────────────────────────────────────────────────────────────┘      │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │                    Launchpad Adapter Layer                           │      │
│  │  • Unified Interface (Pump.fun, Bonk.fun, Generic)                  │      │
│  │  • Safety Validators (Allowlists, Instruction Validation)            │      │
│  │  • Behavior Sanitization                                            │      │
│  └────────────────────────────────────────────────────────────────────┘      │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │                    Execution Engine                                  │      │
│  │  • Order Slicing & Fragmentation                                    │      │
│  │  • Timing Randomization (Gaussian/Uniform)                          │      │
│  │  • MEV Protection (Jito Bundles, Private Routing)                   │      │
│  │  • Transaction Building & Monitoring                                │      │
│  └────────────────────────────────────────────────────────────────────┘      │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │                    Confidential Intel Client                        │      │
│  │  • Arcium Bridge Integration                                        │      │
│  │  • Encrypted Computation Requests                                   │      │
│  │  • Confidential Strategy Planning                                   │      │
│  │  • Multi-Party Computation (MPC)                                     │      │
│  └────────────────────────────────────────────────────────────────────┘      │
└───────────────────────────────┬──────────────────────────────────────────────┘
                                │
                                │ Solana RPC / On-Chain Operations
                                │
┌───────────────────────────────▼──────────────────────────────────────────────┐
│                           ARCIUM LAYER (NEW)                                   │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │         Evalys Confidential Intel MXE (Solana)                      │      │
│  │  • confidential_strategy_plan()                                    │      │
│  │  • confidential_risk_score()                                        │      │
│  │  • confidential_curve_eval()                                        │      │
│  └────────────────────────────────────────────────────────────────────┘      │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │              Arcium MPC Cluster (Off-Chain)                        │      │
│  │  • Encrypted Computation Execution                                  │      │
│  │  • Zero-Knowledge Data Processing                                   │      │
│  │  • Privacy-Preserving Analytics                                     │      │
│  └────────────────────────────────────────────────────────────────────┘      │
└───────────────────────────────┬──────────────────────────────────────────────┘
                                │
                                │ Solana RPC / On-Chain Operations
                                │
┌───────────────────────────────▼──────────────────────────────────────────────┐
│                           BLOCKCHAIN LAYER                                    │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │                         Solana Network                              │      │
│  │  • Mainnet / Devnet RPC Endpoints                                   │      │
│  │  • Transaction Execution                                            │      │
│  │  • On-Chain Data Collection                                         │      │
│  │  • Jito Bundle Submission (MEV Protection)                          │      │
│  └────────────────────────────────────────────────────────────────────┘      │
│                                                                                │
│  ┌────────────────────────────────────────────────────────────────────┐      │
│  │                    Memecoin Launchpads                              │      │
│  │  • Pump.fun Protocol                                                │      │
│  │  • Bonk.fun Protocol                                                │      │
│  │  • Other Bonding Curve Platforms                                    │      │
│  └────────────────────────────────────────────────────────────────────┘      │
└────────────────────────────────────────────────────────────────────────────────┘
```

### Data Flow

```
User Request
    │
    ├─► Privacy Engine ──► Selects Privacy Mode (Normal/Stealth/Max Ghost/Confidential)
    │                          │
    │                          ├─► [Optional] Arcium Bridge ──► Confidential Strategy Planning
    │                          │      │
    │                          │      └─► Arcium MXE ──► MPC Computation ──► Encrypted Plan
    │                          │
    │                          ├─► [Optional] Arcium gMCP ──► Encrypted Intent Processing
    │                          │      │
    │                          │      └─► gMPC MXE ──► Encrypted Strategy ──► Execution Plan
    │                          │
    │                          ├─► Burner Swarm ──► Provides Disposable Wallet
    │                          │
    │                          ├─► Curve Intelligence ──► Analyzes Opportunity
    │                          │      │
    │                          │      ├─► [Optional] Arcium Bridge ──► Confidential Curve Eval
    │                          │      │
    │                          │      └─► [Optional] Arcium gMCP ──► Multi-User Confidential Analytics
    │                          │
    │                          └─► Launchpad Adapter ──► Builds Instructions
    │
    └─► Execution Engine ──► Executes with Privacy Features
                                │
                                ├─► Order Slicing
                                ├─► Timing Jitter
                                ├─► MEV Protection
                                └─► Transaction Monitoring
```

---

## Core Repositories

### 1. evalys-privacy-engine

**Privacy Gradient Engine (PGE)** - The orchestration layer for privacy modes.

**Purpose**: Manages privacy modes (Normal, Stealth, Max Ghost, **Confidential**) and dynamically adjusts privacy features based on transaction context, risk level, and user preferences.

**Key Features**:
- Four-tier privacy gradient system (including Arcium-powered Confidential mode)
- Intelligent mode selection based on risk assessment
- Dynamic privacy level adjustment
- **Arcium integration** for confidential strategy planning
- REST API for integration (Port 8001)
- Standalone operation capability

**Technology**: Python 3.10+, FastAPI, Pydantic

**Status**: ✅ Production Ready

**Repository**: `https://github.com/evalysfun/evalys-privacy-engine`

---

### 2. evalys-burner-swarm

**Burner Swarm Fabric** - Disposable wallet management system.

**Purpose**: Generates, manages, and rotates disposable Solana wallets (burner wallets) for privacy-preserving transactions.

**Key Features**:
- Secure wallet generation with encrypted key storage
- Pool management (Active, Reserve, Retired pools)
- Just-In-Time (JIT) funding capabilities
- Automatic wallet rotation strategies
- REST API for integration (Port 8002)
- Standalone operation capability

**Technology**: Python 3.10+, FastAPI, Solana-py, Solders, PyNaCl

**Status**: ✅ Production Ready

**Repository**: `https://github.com/evalysfun/evalys-burner-swarm`

---

### 3. evalys-curve-intelligence

**Curve Intelligence Layer** - Real-time bonding curve analysis and risk detection.

**Purpose**: Analyzes bonding curves, detects risks, identifies optimal execution windows, and recognizes trading patterns.

**Key Features**:
- Real-time curve analysis (slope, liquidity, volatility)
- Risk detection (sniper activity, buy clusters, liquidity risks)
- Execution window optimization
- Pattern recognition (whale movements, bot activity, pump/dump patterns)
- **Confidential curve evaluation** via Arcium (optional)
- REST API for integration (Port 8003)
- Standalone operation capability

**Technology**: Python 3.10+, FastAPI, NumPy, SciPy

**Status**: ✅ Production Ready

**Repository**: `https://github.com/evalysfun/evalys-curve-intelligence`

---

### 4. evalys-launchpad-adapters

**Launchpad Adapter Layer** - Unified interface for memecoin launchpads.

**Purpose**: Provides a standardized interface for interacting with different memecoin launchpad platforms (Pump.fun, Bonk.fun, and others).

**Key Features**:
- Unified adapter interface for multiple platforms
- Platform-specific adapters (Pump.fun, Bonk.fun, Generic)
- Safety validators (allowlists, instruction validation)
- Behavior sanitization
- REST API for integration (Port 8005)
- Standalone operation capability

**Technology**: Python 3.10+, FastAPI, Solana-py, Solders

**Status**: ✅ Framework Ready (Platform-specific implementations in progress)

**Repository**: `https://github.com/evalysfun/evalys-launchpad-adapters`

---

### 5. evalys-execution-engine

**Execution Engine** - Privacy-preserving transaction execution.

**Purpose**: Executes transactions with privacy-preserving techniques including order slicing, timing randomization, and MEV protection.

**Key Features**:
- Order slicing and fragmentation
- Timing randomization (Gaussian and uniform distributions)
- MEV protection (Jito bundle support, private routing)
- Transaction building and optimization
- Execution monitoring and retry logic
- REST API for integration (Port 8004)
- Standalone operation capability

**Technology**: Python 3.10+, FastAPI, Solana-py, Solders

**Status**: ✅ Production Ready

**Repository**: `https://github.com/evalysfun/evalys-execution-engine`

---

### 6. evalys-web-ui

**Web Dashboard** - User interface for Evalys ecosystem.

**Purpose**: React-based dashboard providing real-time analytics, transaction monitoring, and system configuration.

**Key Features**:
- Real-time token intelligence visualization
- Privacy mode configuration
- Transaction monitoring and history
- Risk scoring and alerts
- System status and health monitoring

**Technology**: React, Redux, JavaScript/TypeScript

**Status**: ✅ In Development

**Repository**: `https://github.com/evalysfun/evalys-web-ui`

---

### 7. integration-examples

**Integration Examples** - Comprehensive examples demonstrating component integration.

**Purpose**: Provides working examples showing how all Evalys components work together.

**Contents**:
- Full flow example (end-to-end integration)
- Stealth buy example (privacy-preserving purchase)
- Curve analysis example (intelligence-driven decisions)
- Privacy orchestration example (mode selection scenarios)

**Status**: ✅ Available

**Repository**: `https://github.com/evalysfun/evalys` (in integration-examples directory)

---

### 8. evalys-confidential-intel-mxe

**Confidential Intel MXE** - Arcium-powered encrypted computation program.

**Purpose**: Solana program (MXE) that provides confidential strategy planning, risk scoring, and curve analytics using Arcium's encrypted supercomputer.

**Key Features**:
- Three encrypted computation functions (strategy plan, risk score, curve eval)
- Arcis-based confidential instructions
- MPC execution via Arcium network
- Zero-knowledge data processing
- Deployed on Solana blockchain

**Technology**: Rust, Arcis, Anchor, Solana

**Status**: ✅ Framework Ready

**Repository**: `https://github.com/evalysfun/evalys-confidential-intel-mxe`

---

### 9. evalys-arcium-bridge-service

**Arcium Bridge Service** - Connects Evalys to Arcium's encrypted supercomputer.

**Purpose**: FastAPI microservice that handles encryption, submits confidential computations to Arcium MXE, and feeds results back to Evalys components.

**Key Features**:
- Client-side encryption of sensitive inputs
- MXE computation submission and monitoring
- Result decryption and processing
- REST API for integration (Port 8010)
- Standalone operation capability

**Technology**: Python 3.10+, FastAPI, Solana-py, Solders

**Status**: ✅ Production Ready

**Repository**: `https://github.com/evalysfun/evalys-arcium-bridge-service`

---

### 10. evalys-arcium-gMCP

**Arcium gMPC Integration** - Confidential compute layer for ghost-mode execution using generalized Multi-Party Computation.

**Purpose**: Specialized bridge service for encrypted intent processing and strategy generation. Enables Evalys to compute user intent, strategy logic, and execution parameters without revealing sensitive data, even during computation.

**Key Features**:
- Encrypted intent processing (user intent → encrypted → computed → execution plan)
- gMPC-powered strategy generation without revealing raw inputs
- Multi-user confidential analytics aggregation
- REST API for integration (Port 8011)
- Works alongside Arcium Bridge Service for different use cases
- Standalone operation capability

**Technology**: Python 3.10+, FastAPI, Rust (Arcis/Anchor for MXE), Solana-py

**Status**: ✅ Framework Ready

**Repository**: `https://github.com/evalysfun/evalys-arcium-gMCP`

---

## System Capabilities

### Privacy Features

- **Four-Tier Privacy Gradient**: Normal, Stealth, Max Ghost, and **Confidential** modes with increasing privacy protection
- **Confidential Computation**: Arcium-powered strategy planning and risk assessment without exposing sensitive data
- **Disposable Wallets**: Burner wallet system for transaction unlinkability
- **Order Fragmentation**: Large orders split into randomized fragments
- **Timing Randomization**: Unpredictable delays to prevent pattern detection
- **MEV Protection**: Jito bundle support and private routing to prevent front-running

### Intelligence Features

- **Real-Time Curve Analysis**: Continuous monitoring of bonding curve dynamics
- **Confidential Analytics**: Arcium-powered curve evaluation and risk scoring with encrypted user context
- **Multi-User Intelligence**: Aggregated insights across users without exposing individual behavior
- **Risk Detection**: Automated identification of sniper activity, buy clusters, and liquidity risks
- **Pattern Recognition**: Detection of whale movements, bot activity, and market anomalies
- **Execution Optimization**: Calculation of optimal execution windows based on market conditions

### Safety Features

- **Allowlist Management**: Whitelist-based safety controls
- **Instruction Validation**: Transaction instruction verification
- **Behavior Sanitization**: Removal of identifying transaction patterns
- **Compliance Tools**: Built-in safety validators and sanitizers

---

## Technology Stack

### Backend
- **Language**: Python 3.10+, Rust (for MXE)
- **Framework**: FastAPI (REST APIs), Anchor (Solana programs)
- **Blockchain**: Solana (solana-py, solders)
- **Confidential Compute**: Arcium (MPC, Arcis DSL)
- **Cryptography**: PyNaCl (encryption), x25519 + Rescue cipher (Arcium)
- **Data Processing**: NumPy, SciPy

### Frontend
- **Framework**: React
- **State Management**: Redux
- **Language**: JavaScript/TypeScript

### Infrastructure
- **Virtual Environment**: Shared Python venv (recommended)
- **API Servers**: Uvicorn (ASGI)
- **Testing**: Pytest, pytest-asyncio
- **Code Quality**: Black, Flake8, MyPy

---

## Getting Started

### Prerequisites

- Python 3.10 or higher
- Node.js 16+ (for web UI)
- Solana CLI tools (optional, for local development)
- Git

### Quick Start

1. **Clone the repositories**:
   ```bash
   git clone https://github.com/evalysfun/evalys-privacy-engine
   git clone https://github.com/evalysfun/evalys-burner-swarm
   git clone https://github.com/evalysfun/evalys-curve-intelligence
   git clone https://github.com/evalysfun/evalys-launchpad-adapters
   git clone https://github.com/evalysfun/evalys-execution-engine
   git clone https://github.com/evalysfun/evalys-arcium-bridge-service
   git clone https://github.com/evalysfun/evalys-arcium-gMCP
   git clone https://github.com/evalysfun/evalys-confidential-intel-mxe
   ```

2. **Set up shared virtual environment** (recommended):
   ```bash
   # From evalys root directory
   python -m venv venv
   
   # Activate virtual environment
   # Windows PowerShell:
   venv\Scripts\Activate.ps1
   $env:PYTHONPATH = "."
   
   # Linux/Mac:
   source venv/bin/activate
   export PYTHONPATH=.
   ```

3. **Install dependencies**:
   ```bash
   cd evalys-privacy-engine
   pip install -r requirements.txt
   pip install -e .
   
   # Repeat for other components
   ```

4. **Run integration examples**:
   ```bash
   python integration-examples/full_flow_example.py
   ```

For detailed setup instructions, see each component's README and QUICKSTART guide.

---

## Architecture Principles

### Modularity
Each component is designed as an independent, reusable module that can operate standalone or integrate with other components.

### Privacy by Design
Privacy features are built into the core architecture, not added as an afterthought. Every transaction respects user privacy.

### Composability
Components communicate through well-defined APIs, enabling flexible system configurations and easy extension.

### Testability
Each component includes comprehensive test suites and can be tested independently.

### Extensibility
The system is designed to accommodate new launchpads, privacy modes, and execution strategies without requiring core changes.

---

## Development Status

| Component | Status | Version | Tests |
|-----------|--------|---------|-------|
| Privacy Engine | ✅ Production Ready | 0.1.0 | ✅ Passing |
| Burner Swarm | ✅ Production Ready | 0.1.0 | ✅ Passing |
| Curve Intelligence | ✅ Production Ready | 0.1.0 | ✅ Passing |
| Launchpad Adapters | ✅ Framework Ready | 0.1.0 | ✅ Passing |
| Execution Engine | ✅ Production Ready | 0.1.0 | ✅ Passing |
| Arcium Bridge Service | ✅ Production Ready | 0.1.0 | ✅ Passing |
| Arcium gMCP | ✅ Framework Ready | 0.1.0 | ✅ Passing |
| Confidential Intel MXE | ✅ Framework Ready | 0.1.0 | - |
| Web UI | 🚧 In Development | - | - |

---

## Documentation

- **Component READMEs**: Each repository includes comprehensive documentation
- **Quick Start Guides**: Step-by-step setup instructions for each component
- **Integration Examples**: Working examples demonstrating component integration
- **Arcium Integration Guide**: Complete guide to Arcium integration (`ARCIUM_INTEGRATION_GUIDE.md`)
- **API Documentation**: Auto-generated API docs available when running each service

---

## Contributing

We welcome contributions! Each component repository includes:
- Contributing guidelines
- Code of conduct
- Issue templates
- Pull request templates

### Contribution Areas

- **Platform Adapters**: Implement adapters for new launchpad platforms
- **Privacy Features**: Enhance privacy protection mechanisms
- **Intelligence Models**: Improve curve analysis and risk detection
- **Documentation**: Improve documentation and examples
- **Testing**: Expand test coverage

---

## License

All Evalys components are licensed under the **MIT License**.

This ensures:
- Open source compliance
- Commercial use permitted
- Community collaboration enabled

See individual repository LICENSE files for details.

---

## Security

Security is a core principle of Evalys. If you discover a security vulnerability, please report it via: **[Twitter](https://x.com/evalysfun)**

**Do not** open public issues for security vulnerabilities.

---

## Support

- **Documentation**: See individual component READMEs
- **Issues**: Open an issue in the relevant repository
- **Discussions**: GitHub Discussions (coming soon)
- **Twitter**: [@evalysfun](https://x.com/evalysfun)

---

## Roadmap

### Phase 1: Core Infrastructure ✅
- [x] Privacy Engine
- [x] Burner Swarm
- [x] Curve Intelligence
- [x] Launchpad Adapters (framework)
- [x] Execution Engine
- [x] Arcium Bridge Service
- [x] Arcium gMCP (framework)
- [x] Confidential Intel MXE (framework)

### Phase 2: Platform Integration 🚧
- [ ] Pump.fun full implementation
- [ ] Bonk.fun full implementation
- [ ] Additional launchpad platforms

### Phase 3: Advanced Features 📋
- [ ] Enhanced MEV protection
- [ ] Advanced pattern recognition
- [ ] Machine learning models for risk prediction
- [ ] Cross-chain support
- [ ] Expanded Arcium MXE computations
- [ ] Multi-user confidential analytics

### Phase 4: Ecosystem 📋
- [ ] SDK development
- [ ] Developer tools
- [ ] Community governance
- [ ] Decentralized deployment options

---

## Citation

If you use Evalys in your research or project, please cite:

```
Evalys: The Privacy Layer for Memecoin Launchpads
https://github.com/evalysfun
```

---

<div align="center">

**Evalys** - Privacy-preserving infrastructure for the future of memecoin trading

*Powered by Arcium's encrypted supercomputer for confidential computation*

[Website](https://evalys.fun) • [Documentation](https://docs.evalys.fun) • [GitHub](https://github.com/evalysfun) • [Arcium Integration Guide](ARCIUM_INTEGRATION_GUIDE.md)

</div>
