# BitVault Pro - Deployment Guide

This guide provides comprehensive instructions for deploying BitVault Pro to various Stacks networks.

## Prerequisites

### System Requirements

- Node.js 18+
- Clarinet CLI 2.0+
- Stacks CLI 5.0+
- Docker (optional, for containerized deployment)

### Wallet Setup

- Stacks wallet with sufficient STX for deployment
- Testnet STX for testing (get from faucet)
- Private key securely stored

## Environment Setup

### 1. Install Dependencies

```bash
# Install Clarinet
curl --proto '=https' --tlsv1.2 -sSf \
  https://raw.githubusercontent.com/hirosystems/clarinet/main/install.sh | sh

# Install Stacks CLI
npm install -g @stacks/cli

# Verify installations
clarinet --version
stx --version
```

### 2. Configure Networks

Edit `Clarinet.toml` for network settings:

```toml
[project]
name = 'bitvault'
description = 'Institutional-Grade Bitcoin L2 Staking Infrastructure'

[contracts.bitvault]
path = 'contracts/bitvault.clar'
clarity_version = 3
epoch = 3.1

[repl.analysis]
passes = ['check_checker']
```

## Local Development

### 1. Start Local Environment

```bash
# Clone repository
git clone https://github.com/hammedibr/bitvault.git
cd bitvault

# Install dependencies
npm install

# Start local Clarinet console
clarinet console
```

### 2. Test Contract Locally

```bash
# Run contract checks
clarinet check

# Execute test suite
npm test

# Test with coverage
npm run test:report
```

### 3. Interactive Testing

```clarity
;; In Clarinet console
::get_accounts
::advance_chain_tip 10

;; Deploy and test
(contract-call? .bitvault initialize-contract)
(contract-call? .bitvault stake-stx u1000000 u0)
```

## Testnet Deployment

### 1. Configure Testnet Settings

Create `settings/Testnet.toml`:

```toml
[network]
name = "testnet"
deployment_fee_rate = 10

[accounts.deployer]
mnemonic = "your testnet mnemonic here"
balance = 100000000

[accounts.wallet_1]
mnemonic = "test wallet mnemonic"
balance = 100000000
```

### 2. Deploy to Testnet

```bash
# Deploy contract
clarinet deployments generate --network=testnet
clarinet deployments apply --network=testnet

# Verify deployment
stx call_read_only_function \
  --network=testnet \
  ST1234567890ABCDEF.bitvault \
  get-contract-owner
```

### 3. Initialize Protocol

```bash
# Initialize contract
stx call_contract_function \
  --network=testnet \
  --private-key=your-private-key \
  ST1234567890ABCDEF.bitvault \
  initialize-contract
```

## Mainnet Deployment

### 1. Security Checklist

- [ ] Smart contract audited by third party
- [ ] All tests passing with 100% coverage
- [ ] Emergency procedures documented
- [ ] Multi-signature setup for admin functions
- [ ] Deployment transaction fees calculated
- [ ] Backup and recovery procedures tested

### 2. Final Pre-deployment Testing

```bash
# Run comprehensive test suite
npm run test:coverage

# Security analysis
clarinet analyze --contracts=bitvault

# Gas estimation
clarinet costs bitvault initialize-contract
```

### 3. Mainnet Configuration

Create `settings/Mainnet.toml`:

```toml
[network]
name = "mainnet"
deployment_fee_rate = 1

[accounts.deployer]
mnemonic = "secure mainnet deployer mnemonic"
```

### 4. Deploy to Mainnet

```bash
# Generate mainnet deployment plan
clarinet deployments generate --network=mainnet

# Review deployment plan carefully
cat deployments/mainnet-deployment-plan.yaml

# Deploy (requires confirmation)
clarinet deployments apply --network=mainnet --broadcast
```

## Post-Deployment Setup

### 1. Contract Initialization

```bash
# Initialize protocol tiers
stx call_contract_function \
  --network=mainnet \
  --private-key=$DEPLOYER_KEY \
  $CONTRACT_ADDRESS.bitvault \
  initialize-contract

# Verify initialization
stx call_read_only_function \
  --network=mainnet \
  $CONTRACT_ADDRESS.bitvault \
  get-stx-pool
```

### 2. Access Control Setup

```bash
# Transfer ownership if needed
stx call_contract_function \
  --network=mainnet \
  --private-key=$DEPLOYER_KEY \
  $CONTRACT_ADDRESS.bitvault \
  transfer-ownership \
  --args="'$NEW_OWNER_ADDRESS"
```

### 3. Monitoring Setup

Configure monitoring for:

- Contract health checks
- Transaction monitoring
- Error alerting
- Performance metrics

## Verification and Testing

### 1. Functional Tests

```bash
# Test basic staking
stx call_contract_function \
  --network=mainnet \
  --private-key=$TEST_KEY \
  $CONTRACT_ADDRESS.bitvault \
  stake-stx \
  --args="u1000000,u0"

# Verify position
stx call_read_only_function \
  --network=mainnet \
  $CONTRACT_ADDRESS.bitvault \
  get-user-position \
  --args="'$TEST_ADDRESS"
```

### 2. Security Verification

```bash
# Test access controls
stx call_contract_function \
  --network=mainnet \
  --private-key=$WRONG_KEY \
  $CONTRACT_ADDRESS.bitvault \
  pause-contract
# Should fail with authorization error

# Test emergency pause
stx call_contract_function \
  --network=mainnet \
  --private-key=$OWNER_KEY \
  $CONTRACT_ADDRESS.bitvault \
  pause-contract
```

## Troubleshooting

### Common Issues

#### Deployment Failures

```bash
# Check network status
stx info

# Verify account balance
stx balance $DEPLOYER_ADDRESS

# Check contract syntax
clarinet check
```

#### Transaction Errors

```bash
# Check transaction status
stx tx_status $TRANSACTION_ID

# View transaction details
stx get_transaction $TRANSACTION_ID
```

#### Contract Interaction Issues

```bash
# Verify contract deployment
stx get_contract_info $CONTRACT_ADDRESS.bitvault

# Check function signatures
stx get_contract_interface $CONTRACT_ADDRESS.bitvault
```

### Error Resolution

| Error | Cause | Solution |
|-------|-------|----------|
| `u1000` | Not authorized | Check private key ownership |
| `u1002` | Invalid amount | Verify minimum stake requirements |
| `u1007` | Contract paused | Wait for unpause or contact admin |

## Upgrade Procedures

### 1. Prepare New Version

```bash
# Test new contract version
clarinet check contracts/bitvault-v2.clar

# Run migration tests
npm run test:migration
```

### 2. Migration Strategy

- Deploy new contract version
- Update frontend to use new contract
- Migrate user positions (if needed)
- Decommission old contract

### 3. Rollback Plan

- Keep old contract as backup
- Document rollback procedures
- Test rollback process
- Monitor for issues

## Maintenance

### Regular Tasks

- Monitor contract health
- Update dependencies
- Review security alerts
- Backup critical data
- Performance optimization

### Security Updates

- Apply security patches promptly
- Review audit recommendations
- Update access controls
- Monitor for suspicious activity

## Support

### Getting Help

- **Documentation**: [docs.bitvault.pro](https://docs.bitvault.pro)
- **Discord**: [Community Support](https://discord.gg/bitvault)
- **GitHub**: [Issue Tracker](https://github.com/hammedibr/bitvault/issues)
- **Email**: <dev@bitvault.pro>

### Emergency Contacts

- **Security Issues**: <security@bitvault.pro>
- **Critical Bugs**: <urgent@bitvault.pro>
- **Infrastructure**: <ops@bitvault.pro>

---

**⚠️ Important**: Always test thoroughly on testnet before mainnet deployment. Keep private keys secure and follow security best practices.
