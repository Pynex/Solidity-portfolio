# Bettum Prediction Market

## Project Overview

A sophisticated prediction market platform enabling users to bet on cryptocurrency price movements with a three-phase round system. Features integration with Uniswap price feeds, custom oracle support, and incentive mechanisms for new users. Demonstrates advanced smart contract architecture for prediction markets and DeFi integration.

## Core Functionality

### Three-Phase Round System

#### Phase 1: Start Round

- **Open Betting** - Users can place bets on price direction (UP/DOWN)
- **Live Price Updates** - Real-time price movements during betting period
- **Bet Aggregation** - Pool accumulation for both directions
- **User Registration** - New user tracking and bonus eligibility

#### Phase 2: Lock Round

- **Betting Closure** - No new bets accepted
- **Price Observation** - Continued price monitoring without betting
- **Final Price Preparation** - System prepares for resolution
- **Anticipation Period** - Users await round resolution

#### Phase 3: End Round

- **Price Locking** - Final price determination and recording
- **Winner Calculation** - Identification of winning direction
- **Reward Distribution** - Proportional payout to winning participants
- **Platform Fee Collection** - Commission extraction for sustainability

## Round Management System

### Automated Round Progression

```solidity
contract Bettum {
    enum RoundPhase { START, LOCK, END }
    
    struct Round {
        uint256 roundId;
        RoundPhase phase;
        uint256 startPrice;
        uint256 lockPrice;
        uint256 endPrice;
        uint256 totalUpAmount;
        uint256 totalDownAmount;
        uint40 startTimestamp;
        uint40 lockTimestamp;
        uint40 endTimestamp;
    }
    
    function startRound() external;
    function lockRound() external; 
    function endRound() external;
    function placeBet(bool _isUp, uint256 _amount) external payable;
}
```

### Phase Transition Logic

- **Automatic Progression** - Time-based phase transitions
- **Manual Override** - Administrative control for emergency situations  
- **State Validation** - Comprehensive phase transition checks
- **Event Emission** - Real-time updates for frontend integration

## Betting & Reward Mechanics

### Betting System

- **Binary Outcomes** - Simple UP/DOWN price predictions
- **Minimum Bet Amount** - Configurable minimum participation
- **Pool Accumulation** - Separate pools for each direction
- **Gas Optimization** - Efficient bet recording and storage

### Reward Calculation

```solidity
function calculateReward(address user, uint256 roundId) public view returns (uint256) {
    // Proportional distribution based on:
    // - User's bet amount
    // - Total winning pool size  
    // - Platform fee deduction
    // - Bonus multipliers (if applicable)
}
```

### Fee Structure

- **Platform Commission** - Configurable percentage on total pool
- **Winner Distribution** - Remaining funds distributed proportionally
- **Gas Optimization** - Batch processing for multiple winners
- **Transparency** - Clear fee calculation and distribution

## Oracle Integration

### Dual Oracle System

#### Uniswap Router Integration

```solidity
// Primary price source for major trading pairs
function getUniswapPrice(address token0, address token1) external view returns (uint256) {
    // Integration with Uniswap V2/V3 router
    // Real-time price data from liquidity pools
    // Price manipulation resistance
}
```

#### Custom Oracle Support

```solidity
// Secondary oracle for additional pairs and validation
contract CustomOracle {
    function updatePrice(bytes32 pairId, uint256 price) external;
    function getLatestPrice(bytes32 pairId) external view returns (uint256);
    function addPriceFeed(bytes32 pairId, uint8 decimals) external;
}
```

### Price Feed Management

- **Multi-Source Validation** - Cross-reference between oracles
- **Fallback Mechanism** - Secondary oracle if primary fails
- **Price Deviation Protection** - Abnormal price movement detection
- **Update Frequency** - Configurable price update intervals

## Incentive System

### New User Bonuses

- **First Bet Bonus** - Enhanced rewards for new participants
- **Qualification Tracking** - Automatic new user identification
- **Bonus Multipliers** - Increased payout percentages
- **One-Time Eligibility** - Prevents bonus farming

### Bonus Implementation

```solidity
mapping(address => bool) public hasClaimedBonus;
uint256 public newUserBonusMultiplier = 2000; // 20% bonus

function calculateBonusReward(address user, uint256 baseReward) internal view returns (uint256) {
    if (!hasClaimedBonus[user]) {
        return (baseReward * newUserBonusMultiplier) / 100;
    }
    return baseReward;
}
```

## 🛠 Technical Implementation

### Technology Stack

- **Framework**: Foundry
- **Language**: Solidity ^0.8.x
- **Libraries**: OpenZeppelin (Security, Access Control)
- **Oracle Integration**: Uniswap Router, Custom Oracle
- **Testing**: Forge with comprehensive test coverage

### Smart Contract Architecture

- **Modular Design** - Separated concerns for betting, oracle, and reward logic
- **Gas Optimization** - Efficient storage patterns and batch operations
- **Security Patterns** - Reentrancy protection and access control
- **Event-Driven** - Comprehensive event emission for frontend integration

### Core Contract Structure

```solidity
contract Bettum {
    // State management
    mapping(uint256 => Round) public rounds;
    mapping(uint256 => mapping(address => Bet)) public bets;
    mapping(address => bool) public hasClaimedBonus;
    
    // Oracle integration
    IUniswapRouter public uniswapRouter;
    ICustomOracle public customOracle;
    
    // Configuration
    uint256 public roundDuration;
    uint256 public lockDuration;
    uint256 public platformFeeRate;
    uint256 public newUserBonusRate;
}
```

## Security & Optimization

### Security Features

- **Access Control** - Administrative functions protected
- **Reentrancy Protection** - Safe external call patterns
- **Price Manipulation Resistance** - Multi-oracle validation
- **Emergency Controls** - Circuit breakers and pause functionality

### Gas Optimization Techniques

- **Packed Structs** - Efficient storage layout
- **Batch Operations** - Multiple rewards in single transaction
- **Minimal Storage Reads** - Cached values for repeated access
- **Event Optimization** - Efficient logging for off-chain indexing

## Testing & Quality Assurance

### Comprehensive Test Suite

- **Core Business Logic** - All betting and reward mechanisms tested
- **Oracle Integration** - Price feed validation and fallback testing
- **Edge Cases** - Boundary conditions and error scenarios
- **Gas Analysis** - Performance optimization verification

### Test Categories

- **Unit Tests** - Individual function validation
- **Integration Tests** - Multi-contract interaction testing
- **Scenario Tests** - End-to-end user journey validation

## Deployment

### Multi-Network Deployment

- **Testnet Validation** - Comprehensive testing on multiple testnets
- **Network Configuration** - Flexible oracle and router addresses
- **Contract Verification** - Automated source code verification

## Economic Model

### Revenue Streams

- **Platform Fees** - Percentage of total betting pools
- **Bonus Redistribution** - Unclaimed bonus reallocation

## System Workflow

### Complete User Journey

1. **Round Start** - New betting round begins with initial price
2. **Place Bets** - Users predict price direction (UP/DOWN)
3. **Round Lock** - Betting closes, price continues updating
4. **Round End** - Final price locked, winners determined
5. **Claim Rewards** - Winners collect proportional payouts
6. **Bonus Application** - New users receive enhanced rewards

### Administrative Operations

- **Round Management** - Manual override capabilities
- **Oracle Updates** - Price feed management and validation
- **Parameter Adjustment** - Fee rates and bonus configuration
- **Emergency Controls** - Pause/unpause functionality
