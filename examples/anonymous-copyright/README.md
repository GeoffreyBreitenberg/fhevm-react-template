# Anonymous Copyright Protection System

A production-ready dApp demonstrating FHEVM integration for anonymous copyright registration and management.

## 🎯 Overview

This example showcases a complete implementation of Fully Homomorphic Encryption (FHE) for protecting copyright while maintaining author anonymity. Built with Zama's FHEVM technology on Ethereum Sepolia.

### Key Features

🔒 **Encrypted Content Hashes** - Content hashes stored with euint32 FHE encryption
👤 **Anonymous Author IDs** - Author identities protected with euint64 encryption
⚖️ **Dispute Management** - Copyright dispute resolution with encrypted proofs
🛡️ **Access Control** - Owner-based permission system
✅ **Production Ready** - 56+ tests, CI/CD, security auditing

---

## 🚀 Quick Start

### Prerequisites

- Node.js ≥ 18.0.0
- npm ≥ 9.0.0
- Sepolia ETH (for deployment)

### Installation

```bash
# Navigate to example
cd examples/anonymous-copyright

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your keys
```

### Development

```bash
# Compile contracts
npm run compile

# Run tests
npm test

# Check coverage
npm run coverage

# Deploy to Sepolia
npm run deploy
```

---

## 📁 Project Structure

```
anonymous-copyright/
├── contracts/
│   └── AnonymousCopyright.sol        # Main contract with FHEVM
├── scripts/
│   ├── deploy.js                     # Deployment script
│   ├── verify.js                     # Etherscan verification
│   ├── interact.js                   # Interaction examples
│   └── simulate.js                   # Full simulation
├── test/
│   ├── AnonymousCopyright.test.js    # 56 test cases
│   └── AnonymousCopyright.sepolia.test.js
├── .github/workflows/                # CI/CD pipelines
├── hardhat.config.js                 # Hardhat configuration
└── package.json                      # Dependencies & scripts
```

---

## 📖 Smart Contract

### Contract: AnonymousCopyright.sol

**Location**: `contracts/AnonymousCopyright.sol`
**Solidity**: 0.8.24
**Libraries**: @fhevm/solidity

### Core Functions

#### 1. Author Registration

```solidity
function registerAuthor(uint64 _authorId) external
```

Registers user as author with encrypted ID.

**Parameters**:
- `_authorId`: Author's unique identifier (encrypted with euint64)

**Events**: `AuthorRegistered(address indexed author, uint256 timestamp)`

#### 2. Work Registration

```solidity
function registerWork(
    uint32 _contentHash,
    string calldata _title,
    string calldata _category
) external returns (uint256)
```

Registers original work with encrypted content hash.

**Parameters**:
- `_contentHash`: Content hash (encrypted with euint32)
- `_title`: Work title (public)
- `_category`: Work category (public)

**Returns**: Work ID
**Events**: `WorkRegistered(uint256 indexed workId, address indexed registrant, string title, uint256 timestamp)`

#### 3. Dispute Filing

```solidity
function fileDispute(
    uint256 _workId,
    uint32 _challengerContentHash
) external
```

Files copyright dispute with encrypted proof.

**Parameters**:
- `_workId`: Work to dispute
- `_challengerContentHash`: Challenger's content hash (encrypted)

**Events**: `DisputeFiled(uint256 indexed workId, address indexed challenger, uint256 disputeId)`

#### 4. Work Verification

```solidity
function requestVerifyWork(
    uint256 _workId,
    uint32 _contentHashToVerify
) external
```

Verifies work ownership using encrypted comparison.

**Parameters**:
- `_workId`: Work to verify
- `_contentHashToVerify`: Hash to compare (encrypted)

---

## 🧪 Testing

### Test Suite: 56 Tests

**Categories**:
- Deployment & Initialization (5 tests)
- Author Registration (8 tests)
- Work Registration (10 tests)
- Work Verification (7 tests)
- Dispute Management (8 tests)
- View Functions (4 tests)
- Access Control (5 tests)
- Edge Cases (6 tests)
- Gas Optimization (3 tests)

### Run Tests

```bash
# All tests
npm test

# Main suite only
npm run test:main

# With gas reporting
npm run test:gas

# Coverage report
npm run coverage
```

### Expected Coverage

- Statements: ≥ 85%
- Branches: ≥ 75%
- Functions: ≥ 90%
- Lines: ≥ 85%

---

## 🌐 Deployment

### Deploy to Sepolia

```bash
# Configure .env first
npm run deploy
```

### Verify on Etherscan

```bash
npm run verify
```

### Interact with Contract

```bash
# Run interaction examples
npm run interact

# Run full simulation
npm run simulate
```

---

## 🔐 Security Features

### Contract Security

✅ Access control with `onlyOwner` modifier
✅ Input validation on all functions
✅ FHE encryption for sensitive data
✅ Event logging for transparency
✅ Reentrancy protection

### Development Security

✅ Pre-commit hooks (Husky)
✅ Security audit script
✅ Dependency vulnerability scanning
✅ Solhint linting
✅ Gas optimization

### Run Security Audit

```bash
npm run security
```

---

## ⚡ Performance

### Gas Costs

| Function | Gas Used | Status |
|----------|----------|--------|
| registerAuthor | ~187,000 | ✅ Optimized |
| registerWork | ~257,000 | ✅ Optimized |
| fileDispute | ~205,000 | ✅ Optimized |
| markWorkAsVerified | ~47,000 | ✅ Optimized |

### Gas Analysis

```bash
npm run gas:analysis
```

---

## 📚 Available Scripts

### Development

```bash
npm run compile          # Compile contracts
npm run clean            # Clean artifacts
npm test                 # Run all tests
npm run coverage         # Generate coverage
```

### Deployment

```bash
npm run deploy           # Deploy to Sepolia
npm run deploy:local     # Deploy to localhost
npm run verify           # Verify on Etherscan
npm run interact         # Interact with deployed contract
npm run simulate         # Run full simulation
```

### Code Quality

```bash
npm run lint             # Lint Solidity + JavaScript
npm run lint:sol         # Lint Solidity only
npm run lint:js          # Lint JavaScript only
npm run format           # Format all code
npm run prettier:check   # Check formatting
```

### Security

```bash
npm run security         # Full security audit
npm run security:audit   # Run audit script
npm run security:check   # npm audit
```

### Performance

```bash
npm run gas              # Gas analysis + reporting
npm run gas:analysis     # Gas analysis only
npm run test:gas         # Tests with gas reporting
```

### CI/CD

```bash
npm run ci               # Standard CI pipeline
npm run ci:full          # Complete validation
```

---

## 🏗️ Architecture

### Data Flow

```
1. User Registration
   User → registerAuthor(encryptedID) → Contract

2. Work Registration
   User → registerWork(encryptedHash, title, category) → Contract

3. Verification
   User → requestVerifyWork(workId, encryptedHash) → Contract
   Contract → FHE Comparison → Result

4. Dispute
   Challenger → fileDispute(workId, encryptedProof) → Contract
```

### FHE Integration

```solidity
// Encrypt author ID
euint64 encryptedAuthorId = FHE.asEuint64(_authorId);

// Encrypt content hash
euint32 encryptedContentHash = FHE.asEuint32(_contentHash);

// Encrypted comparison
ebool isMatch = FHE.eq(work.encryptedContentHash, providedHash);

// Request decryption
FHE.requestDecryption(cts, callback);
```

---

## 📊 Example Usage

### Register as Author

```javascript
const authorId = 123456;
const tx = await contract.registerAuthor(authorId);
await tx.wait();
```

### Register Work

```javascript
const contentHash = 987654321;
const tx = await contract.registerWork(
  contentHash,
  "My Original Work",
  "Digital Art"
);
const receipt = await tx.wait();
const workId = receipt.logs[0].args.workId;
```

### File Dispute

```javascript
const challengerHash = 111222333;
const tx = await contract.fileDispute(workId, challengerHash);
await tx.wait();
```

### Get Work Info

```javascript
const workInfo = await contract.getWorkInfo(workId);
console.log(workInfo.title);        // "My Original Work"
console.log(workInfo.category);     // "Digital Art"
console.log(workInfo.registrant);   // 0x...
console.log(workInfo.verified);     // false
console.log(workInfo.disputed);     // false
```

---

## 🔗 Documentation

- [Main README](../../README.md) - SDK overview
- [DEPLOYMENT.md](./DEPLOYMENT.md) - Deployment guide
- [TESTING.md](./TESTING.md) - Testing documentation
- [SECURITY.md](./SECURITY.md) - Security guide
- [CI_CD.md](./CI_CD.md) - CI/CD documentation

---

## 🤝 Contributing

See main [Contributing Guide](../../CONTRIBUTING.md)

---

## 📄 License

MIT License - see [LICENSE](../../LICENSE)

---

**Part of the Universal FHEVM SDK Project**

Built with ❤️ for the FHEVM community
