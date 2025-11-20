# Evalys + Arcium Integration Guide

**Complete guide to integrating Arcium's encrypted supercomputer with Evalys.**

## Overview

Evalys now integrates with Arcium to provide **confidential computation** for strategy planning, risk assessment, and curve analytics. This enables privacy-preserving intelligence without exposing sensitive user data (wallet graphs, PnL history, alpha signals).

Evalys supports two Arcium integration paths:
- **Arcium Bridge Service**: General confidential computation for strategy planning, risk scoring, and curve evaluation
- **Arcium gMPC**: Specialized generalized Multi-Party Computation for encrypted intent processing and advanced confidential analytics

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Evalys Components                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │   Privacy    │  │    Curve     │  │  Execution   │     │
│  │   Engine     │  │ Intelligence │  │   Engine     │     │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘     │
│         │                 │                 │              │
│         └─────────────────┼─────────────────┘              │
│                           │                                 │
└───────────────────────────┼─────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                     │
        ▼                   ▼                     ▼
┌──────────────┐  ┌──────────────────┐  ┌──────────────────┐
│ Arcium       │  │ Arcium gMPC      │  │ (Direct MXE      │
│ Bridge       │  │ Bridge           │  │  Access)         │
│ (Port 8010)  │  │ (Port 8011)      │  │                  │
└──────┬───────┘  └────────┬─────────┘  └──────────────────┘
       │                   │
       │                   │
       ▼                   ▼
┌─────────────────────────────────────────────────────────────┐
│         Evalys Confidential Intel MXE (Solana)             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  • confidential_strategy_plan()                      │  │
│  │  • confidential_risk_score()                         │  │
│  │  • confidential_curve_eval()                         │  │
│  └──────────────────────────────────────────────────────┘  │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│         Evalys gMPC Strategy MXE (Solana)                  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  • evalys_gmpc_strategy() - Encrypted intent → plan  │  │
│  │  • confidential_multi_user_analytics()                │  │
│  └──────────────────────────────────────────────────────┘  │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│              Arcium MPC Cluster (Off-Chain)                 │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  • Executes encrypted computations                  │  │
│  │  • Returns encrypted results                        │  │
│  │  • Never exposes raw data                           │  │
│  │  • Data encrypted even during computation (gMPC)     │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## Components

### 1. evalys-confidential-intel-mxe

**Rust/Arcis MXE program** deployed on Solana that defines encrypted computation functions.

**Location**: `evalys-confidential-intel-mxe/`

**Key Files**:
- `encrypted-ixs/confidential_strategy.rs` - Strategy planning computation
- `encrypted-ixs/confidential_risk.rs` - Risk scoring computation
- `encrypted-ixs/confidential_curve.rs` - Curve evaluation computation
- `programs/evalys-confidential-intel/` - Solana program that invokes encrypted instructions

**Deployment**:
```bash
cd evalys-confidential-intel-mxe
arcium deploy \
  --cluster-offset 1078779259 \
  --keypair-path ~/.config/solana/id.json \
  --rpc-url https://devnet.helius-rpc.com/?api-key=<your-key>
```

### 2. evalys-arcium-bridge-service

**Python FastAPI service** that bridges Evalys to Arcium for general confidential computation.

**Location**: `evalys-arcium-bridge-service/`

**API Endpoints**:
- `POST /api/v1/arcium/plan` - Get confidential execution plan
- `POST /api/v1/arcium/risk-score` - Get confidential risk assessment
- `POST /api/v1/arcium/curve-eval` - Get confidential curve evaluation
- `GET /api/v1/health` - Health check

**Running**:
```bash
cd evalys-arcium-bridge-service
python -m src.api.server
```

**Use Cases**:
- Strategy planning with user preferences and history
- Risk scoring with portfolio context
- Curve evaluation with user constraints

### 3. evalys-arcium-gMCP

**Python FastAPI service** that bridges Evalys to Arcium gMPC for encrypted intent processing and advanced confidential analytics.

**Location**: `evalys-arcium-gMCP/`

**API Endpoints**:
- `POST /api/v1/gmpc/plan` - Get confidential execution plan from encrypted intent
- `POST /api/v1/gmpc/multi-user-intel` - Multi-user confidential analytics
- `GET /api/v1/health` - Health check

**Running**:
```bash
cd evalys-arcium-gMCP
uvicorn src.api.server:app --reload --port 8011
```

**Use Cases**:
- Encrypted intent processing (user intent → encrypted → computed → plan)
- Confidential strategy generation without revealing raw inputs
- Multi-user analytics aggregation without exposing individual behavior
- Advanced privacy where data remains encrypted even during computation

**Key Difference from Arcium Bridge Service**:
- **Arcium Bridge Service**: General confidential computation for strategy, risk, and curve analysis
- **gMPC Bridge Service**: Specialized for encrypted intent processing and multi-party analytics

Both services can work together or independently based on requirements.

### 4. Updated Components

#### evalys-privacy-engine

**New Features**:
- `PrivacyMode.CONFIDENTIAL` - Arcium-powered confidential mode
- `PrivacyLevel.use_arcium` - Flag indicating Arcium usage
- `PrivacyLevel.arcium_plan_id` - Plan ID from Arcium MXE
- `PrivacyGradientEngine.select_mode()` with `enable_arcium` parameter

**Usage with Arcium Bridge Service**:
```python
from src.pge.orchestrator import PrivacyGradientEngine

pge = PrivacyGradientEngine()
privacy_config = pge.select_mode(
    user_preference="confidential",
    enable_arcium=True,
    arcium_inputs={
        "user_preferences": {...},
        "user_history": {...},
        "curve_state": {...},
    }
)
```

**Usage with gMPC Bridge Service**:
```python
from src.pge.orchestrator import PrivacyGradientEngine

pge = PrivacyGradientEngine()
privacy_config = pge.select_mode(
    user_preference="gmcp",
    use_gmcp=True,
    gmcp_inputs={
        "trader_profile_id": "anon-hash-abc123",
        "token_mint": "GHOSTCAT_MINT",
        "launchpad": "pumpfun",
        "max_size_sol": 2.0,
        "risk_level": "normal",
        "privacy_priority": "max_privacy",
        "market_snapshot": {
            "price": 0.001,
            "liquidity": 100000.0,
            "fdv": 500000.0,
            "curve_position": 0.3,
            "recent_volatility": 0.65
        },
        "historical_stats": {
            "avg_hold_time": 1800.0,
            "win_rate": 0.65,
            "max_dd": 0.15
        }
    }
)
```

#### evalys-curve-intelligence

**New Features**:
- `use_confidential_intel` parameter in `CurveIntelligenceLayer`
- `get_confidential_curve_evaluation()` method for Arcium-powered analytics

**Usage**:
```python
from src.curve_intelligence.intelligence_layer import CurveIntelligenceLayer

curve_intel = CurveIntelligenceLayer(use_confidential_intel=True)
recommendation = await curve_intel.get_confidential_curve_evaluation(
    token_mint=token_mint,
    sizing_preferences={...},
    user_constraints={...},
)
```

## Integration Flows

### Flow A: Confidential Entry with Arcium Shield (Arcium Bridge Service)

1. **User enables Arcium Shield** in UI
2. **Privacy Engine** collects sensitive inputs (preferences, history)
3. **Arcium Bridge Service** encrypts inputs and submits to MXE
4. **Arcium MXE** computes strategy plan confidentially
5. **Privacy Engine** receives plan and configures execution
6. **Execution Engine** executes using confidential plan parameters

### Flow B: Confidential Entry with gMPC (gMPC Bridge Service)

1. **User enables gMPC mode** in UI
2. **Privacy Engine** collects encrypted intent (trader profile, preferences, market snapshot)
3. **gMPC Bridge Service** processes encrypted intent via gMPC MXE
4. **gMPC MXE** computes execution plan from encrypted inputs (data remains encrypted during computation)
5. **Privacy Engine** receives sanitized plan (no raw inputs revealed)
6. **Execution Engine** executes using plan parameters

### Flow C: Confidential Multi-User Analytics (Arcium Bridge Service)

1. **Multiple users** opt in to Arcium-powered intel
2. **Evalys** ships encrypted per-user stats to shared MXE
3. **Arcium MPC** computes aggregated insights without exposing individual data
4. **Curve Intelligence** consumes aggregated metrics
5. **All users** benefit from "big brain intel" without privacy loss

### Flow D: Multi-User Confidential Analytics (gMPC Bridge Service)

1. **Multiple users** opt in to gMPC-powered analytics
2. **Evalys** ships encrypted per-user profiles to gMPC MXE
3. **gMPC MPC** computes aggregated insights (market sentiment, curve patterns) without exposing individual behavior
4. **Curve Intelligence** consumes only aggregated metrics
5. **All users** benefit from collective intelligence without privacy compromise

## Configuration

### Arcium Bridge Service Configuration

Create `.env` in `evalys-arcium-bridge-service/`:

```env
ARCIUM_MXE_PROGRAM_ID=your_mxe_program_id_here
ARCIUM_CLUSTER_OFFSET=1078779259
ARCIUM_RPC_URL=https://api.devnet.solana.com
SOLANA_RPC_URL=https://api.devnet.solana.com
API_PORT=8010
```

### gMPC Bridge Service Configuration

Create `.env` in `evalys-arcium-gMCP/`:

```env
ARCIUM_MXE_ENDPOINT=https://arcium-api.example.com
ARCIUM_MXE_PROGRAM_ID=your_gmpc_mxe_program_id_here
ARCIUM_API_KEY=your_arcium_api_key_here
ENCRYPTION_PRIVATE_KEY=your_encryption_private_key
ENCRYPTION_PUBLIC_KEY=your_encryption_public_key
SOLANA_RPC_URL=https://api.devnet.solana.com
SOLANA_NETWORK=devnet
GMPC_BRIDGE_PORT=8011
LOG_LEVEL=INFO
```

### Privacy Engine Configuration

The Privacy Engine automatically detects both Arcium Bridge Service and gMPC Bridge Service availability. No additional configuration needed if services are running.

**Service Priority**:
- If `use_gmcp=True`, gMPC Bridge Service is used (takes priority)
- If `enable_arcium=True`, Arcium Bridge Service is used
- If both are unavailable, falls back to standard privacy modes

## Example Usage

### Arcium Bridge Service Examples

- **`integration-examples/arcium_confidential_example.py`** - Confidential entry with Arcium Shield
- **`integration-examples/arcium_multi_user_intel_example.py`** - Multi-user confidential analytics

### gMPC Bridge Service Examples

- **`integration-examples/gmcp_confidential_intent_example.py`** - Encrypted intent processing with gMPC
- **`integration-examples/gmcp_multi_user_analytics_example.py`** - Multi-user analytics via gMPC

### Choosing Between Services

**Use Arcium Bridge Service when**:
- You need general confidential computation (strategy, risk, curve analysis)
- You want to use existing user preferences/history format
- You need risk scoring or curve evaluation

**Use gMPC Bridge Service when**:
- You need encrypted intent processing (intent → encrypted → plan)
- You want advanced multi-party analytics
- You need data to remain encrypted even during computation
- You want to process trader profiles and market snapshots confidentially

## Security Model

### Arcium Bridge Service

- **Input Encryption**: All sensitive inputs encrypted using x25519 + Rescue cipher
- **MPC Execution**: Computations run in Arcium's MPC network (Cerberus protocol)
- **Output Encryption**: Results encrypted and only decryptable by requesting client
- **No Data Exposure**: Raw sensitive data never leaves encrypted form

### gMPC Bridge Service

- **Encrypted Intent Processing**: User intent encrypted before submission
- **Data in Use Encryption**: Data remains encrypted even during computation (gMPC)
- **Multi-Party Computation**: Computations distributed across MPC nodes
- **Sanitized Outputs**: Only execution plans returned, no raw inputs revealed
- **Zero-Knowledge Analytics**: Multi-user insights computed without exposing individual behavior

## Positioning

**One-liner for docs**:

> Evalys now plugs into Arcium's encrypted supercomputer – your trading intent, risk profile, and strategy are computed confidentially via MPC, while Evalys executes the resulting plan using burner swarms, MEV-safe routing, and launchpad adapters on Solana.

**gMPC-specific positioning**:

> Evalys integrates Arcium's generalized Multi-Party Computation (gMPC) for encrypted intent processing – your trading intent is processed confidentially, with data remaining encrypted even during computation, enabling advanced privacy-preserving strategy generation and multi-user analytics.

## Next Steps

### For Arcium Bridge Service

1. Deploy `evalys-confidential-intel-mxe` to Solana devnet
2. Start `evalys-arcium-bridge-service` on port 8010
3. Enable Arcium Shield in Privacy Engine (`enable_arcium=True`)
4. Test with `arcium_confidential_example.py`
5. Deploy to production

### For gMPC Bridge Service

1. Deploy `evalys-gmpc-strategy` MXE to Solana devnet (from `evalys-arcium-gMCP/mxe`)
2. Start `evalys-arcium-gMCP` bridge service on port 8011
3. Enable gMPC mode in Privacy Engine (`use_gmcp=True`)
4. Test with `gmcp_confidential_intent_example.py`
5. Deploy to production

### Using Both Services

Both services can run simultaneously and be used for different use cases:
- Use Arcium Bridge Service for general confidential computation
- Use gMPC Bridge Service for encrypted intent processing and advanced analytics

## Support

- Evalys: [GitHub Issues](https://github.com/evalysfun)
- Arcium: [Discord](https://discord.gg/arcium)

