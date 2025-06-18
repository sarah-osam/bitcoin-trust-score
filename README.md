# Bitcoin Trust Score Protocol

A decentralized reputation and trust scoring system for Bitcoin ecosystem participants, built on Stacks blockchain infrastructure.

## 🎯 Overview

The Bitcoin Trust Score Protocol enables trustless reputation management through blockchain-verified actions, supporting governance participation, contract fulfillment, community contributions, and network validation activities. The system features automated reputation decay, configurable scoring multipliers, and comprehensive audit trails to foster trust and accountability in decentralized Bitcoin applications.

## ✨ Key Features

- **Decentralized Identity Management**: Create and manage blockchain-verified identities with unique DIDs
- **Dynamic Reputation Scoring**: Earn reputation through verified on-chain activities
- **Temporal Decay System**: Automatic reputation degradation to maintain score relevance
- **Configurable Action Types**: Customizable reputation actions with adjustable multipliers
- **Comprehensive Audit Trail**: Immutable history of all reputation changes
- **Access Control**: Role-based permissions for administrative functions
- **Threshold Verification**: Built-in reputation verification for minimum score requirements

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    BTSP Smart Contract                     │
├─────────────────────────────────────────────────────────────┤
│  Identity Layer                                             │
│  ┌─────────────────┐  ┌─────────────────┐                  │
│  │   Identities    │  │ Reputation      │                  │
│  │   Registry      │  │ History         │                  │
│  └─────────────────┘  └─────────────────┘                  │
├─────────────────────────────────────────────────────────────┤
│  Reputation Engine                                          │
│  ┌─────────────────┐  ┌─────────────────┐                  │
│  │ Action Types    │  │ Scoring Engine  │                  │
│  │ Configuration   │  │ & Decay System  │                  │
│  └─────────────────┘  └─────────────────┘                  │
├─────────────────────────────────────────────────────────────┤
│  Administration Layer                                       │
│  ┌─────────────────┐  ┌─────────────────┐                  │
│  │ Access Control  │  │ System Config   │                  │
│  │ & Permissions   │  │ Management      │                  │
│  └─────────────────┘  └─────────────────┘                  │
└─────────────────────────────────────────────────────────────┘
```

## 📊 Contract Architecture

### Core Data Structures

#### Identities Map

```clarity
{ owner: principal } → {
  did: string-ascii,
  reputation-score: uint,
  created-at: uint,
  last-updated: uint,
  last-decay: uint,
  total-actions: uint,
  active: bool
}
```

#### Reputation Actions Map

```clarity
{ action-type: string-ascii } → {
  multiplier: uint,
  description: string-ascii,
  active: bool
}
```

#### Reputation History Map

```clarity
{ owner: principal, tx-id: uint } → {
  action-type: string-ascii,
  previous-score: uint,
  new-score: uint,
  timestamp: uint,
  block-height: uint
}
```

## 🔄 Data Flow

### Identity Creation Flow

```
User Request → Validate DID → Check Existing Identity → 
Create Identity Record → Set Initial Reputation → Return DID
```

### Reputation Update Flow

```
Action Request → Validate Identity → Check Action Type → 
Apply Decay (if needed) → Calculate New Score → 
Update Identity → Log History → Return New Score
```

### Decay Process Flow

```
Time Check → Calculate Decay Amount → Apply Reduction → 
Update Last Decay Timestamp → Log Decay Event
```

## 🚀 Getting Started

### Prerequisites

- Stacks blockchain development environment
- Clarinet CLI tool
- Basic understanding of Clarity smart contracts

### Installation

1. Clone the repository:

```bash
git clone https://github.com/sarah-osam/bitcoin-trust-score.git
cd bitcoin-trust-score
```

2. Install dependencies:

```bash
clarinet install
```

3. Run tests:

```bash
clarinet test
```

4. Deploy to testnet:

```bash
clarinet deploy --testnet
```

## 📖 Usage

### Creating an Identity

```clarity
(contract-call? .btsp create-identity "your-unique-did-identifier")
```

### Updating Reputation Score

```clarity
(contract-call? .btsp update-reputation-score "governance-vote")
```

### Checking Reputation

```clarity
(contract-call? .btsp get-reputation 'SP1234567890ABCDEF...)
```

### Verifying Minimum Reputation

```clarity
(contract-call? .btsp verify-reputation 'SP1234567890ABCDEF... u100)
```

## 🎛️ Configuration

### Default Reputation Actions

| Action Type | Multiplier | Description |
|-------------|------------|-------------|
| `governance-vote` | 5 | Participation in governance voting |
| `contract-fulfillment` | 10 | Successful smart contract completion |
| `community-contribution` | 7 | Community project contributions |
| `validation` | 3 | Network transaction validation |
| `content-creation` | 6 | Valuable platform content creation |

### System Parameters

- **Maximum Reputation**: 1000 points
- **Minimum Reputation**: 0 points
- **Starting Reputation**: 50 points (configurable)
- **Default Decay Rate**: 10% per decay period
- **Default Decay Period**: 10,000 blocks

## 🔧 Administrative Functions

### Adding New Action Types

```clarity
(contract-call? .btsp add-reputation-action 
  "new-action-type" 
  u15 
  "Description of the new action")
```

### Updating System Parameters

```clarity
(contract-call? .btsp set-decay-parameters u5 u20000)
```

### Contract Management

```clarity
(contract-call? .btsp set-contract-active false)
```

## 🛡️ Security Considerations

- **Access Control**: Only contract owner can modify system parameters
- **Identity Validation**: DID length validation prevents empty identities
- **Score Bounds**: Reputation scores are capped at maximum values
- **Active State Checks**: All operations verify contract and identity active status
- **Decay Protection**: Time-based validation prevents premature decay application

## 🧪 Testing

The contract includes comprehensive test coverage for:

- Identity creation and management
- Reputation scoring and decay mechanisms
- Administrative function access control
- Edge cases and error conditions
- Historical data integrity

## 📈 Integration Examples

### DeFi Protocol Integration

```clarity
;; Verify user reputation before allowing high-value operations
(let ((user-reputation (unwrap! (contract-call? .btsp get-reputation tx-sender) 
                                 (err u404))))
  (asserts! (>= user-reputation u500) (err u403))
  ;; Proceed with high-trust operation
)
```

### Governance System Integration

```clarity
;; Award reputation for governance participation
(contract-call? .btsp update-reputation-score "governance-vote")
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

For support and questions:

- Create an issue in the GitHub repository
- Join our community Discord
- Check the documentation wiki

## 🗺️ Roadmap

- [ ] Multi-signature administrative controls
- [ ] Reputation delegation mechanisms
- [ ] Cross-chain reputation portability
- [ ] Advanced analytics dashboard
- [ ] Reputation-based token rewards
- [ ] Integration with Bitcoin Lightning Network
