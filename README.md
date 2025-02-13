# Capstone project - TURBIN3 Q1 2025

Capstone project for Turbin 2025 Q1

[LOI](https://docs.google.com/document/d/1RnwgPXAF4HwqqXloj5LX-fdnMdOWr92yRmsojCze8H4/edit?tab=t.0)

## Front End

TBD

## Sequence Diagram

```mermaid
sequenceDiagram
  autonumber
  actor User
  participant Protocol
  participant AIAgent as AI Agent
  participant StakingPlatform
  participant StakingProvider as Staking Provider
  participant LotteryContract

  User ->> Protocol: Request Staking
  AIAgent ->> StakingPlatform: Analyze Platforms
  AIAgent -->> Protocol: Recommends Staking Platform

  Protocol ->> StakingPlatform: Request to Stake SOL
  StakingPlatform ->> StakingProvider: Stake SOL
  StakingProvider -->> StakingPlatform: Confirm Staking
  StakingPlatform -->> User: Staking Confirmed

  Note over User, StakingPlatform: Time passes...

  StakingProvider ->> StakingPlatform: Distribute Staking Rewards
  StakingPlatform -->> User: Notify Reward Distributed

  User ->> StakingPlatform: Request to Bet with Rewards
  StakingPlatform ->> StakingProvider: Verify Rewards Availability
  StakingProvider -->> StakingPlatform: Rewards Verified

  StakingPlatform ->> LotteryContract: Place Bet with Rewards
  LotteryContract -->> StakingPlatform: Confirm Bet
  StakingPlatform -->> User: Bet Placed Successfully

  Note over User, LotteryContract: Betting only allows rewards, not the principal staked amount.
```

## ER

```mermaid
erDiagram
  %% User Account
  UserAccount {
    string user_id
    string wallet_address
    string owner
    string account_type "User Wallet (Token Account)"
  }

  %% Protocol Account
  ProtocolAccount {
    string protocol_id
    string name
    string owner
    string account_type "PDA"
  }

  %% AI Agent (Not an on-chain account, but metadata)
  AIAgent {
    string agent_id
    string model_version
    string owner "Protocol"
  }

  %% Staking Platform Account
  StakingPlatformAccount {
    string platform_id
    string name
    string owner
    string account_type "PDA"
  }

  %% Staking Provider Account
  StakingProviderAccount {
    string provider_id
    string name
    float total_staked
    string owner
    string account_type "PDA"
  }

  %% Lottery Contract Account
  LotteryContractAccount {
    string contract_id
    string name
    string owner
    string account_type "PDA"
  }

  %% Staking Pool Token Account (Holds staked SOL)
  StakingPoolTokenAccount {
    string account_id
    float balance
    string owner "StakingProvider"
    string account_type "Token Account (SOL)"
  }

  %% Rewards Token Account (Holds staking rewards)
  RewardsTokenAccount {
    string account_id
    float balance
    string owner "User"
    string account_type "Token Account (Reward Tokens)"
  }

  %% Relationships
  UserAccount ||--o{ ProtocolAccount: "requests staking"
  ProtocolAccount ||--o{ AIAgent: "consults AI"
  AIAgent ||--o{ StakingPlatformAccount: "analyzes platforms"
  ProtocolAccount ||--o{ StakingPlatformAccount: "stakes via"
  StakingPlatformAccount ||--o{ StakingProviderAccount: "delegates stake"
  StakingProviderAccount ||--o{ StakingPoolTokenAccount: "manages staked SOL"
  StakingProviderAccount ||--o{ StakingPlatformAccount: "confirms staking"
  StakingProviderAccount ||--o{ RewardsTokenAccount: "distributes rewards"
  StakingPlatformAccount ||--o{ UserAccount: "notifies reward"
  UserAccount ||--o{ RewardsTokenAccount: "receives rewards"
  UserAccount ||--o{ StakingPlatformAccount: "bets with rewards"
  StakingPlatformAccount ||--o{ StakingProviderAccount: "verifies rewards"
  StakingPlatformAccount ||--o{ LotteryContractAccount: "places bet"
  LotteryContractAccount ||--o{ StakingPlatformAccount: "confirms bet"
```


## interaction diagram

```mermaid
graph TD

%% User Interaction Flow
  User(User) -->|Deposits SOL| DepositProcess
  DepositProcess --> StakingPlatform(Staking Platform)
  StakingPlatform -->|Confirms Deposit| User

  User -->|Requests Staking| StakingProcess
  StakingProcess --> StakingPlatform
  StakingPlatform --> StakingProvider(Staking Provider)
  StakingProvider -->|Stake Confirmed| StakingPlatform
  StakingPlatform -->|Notifies User| User

  User -->|Claims Rewards| RewardClaiming
  RewardClaiming --> StakingPlatform
  StakingPlatform --> StakingProvider
  StakingProvider --> RewardsTokenAccount(Rewards Token Account)
  RewardsTokenAccount -->|Tokens Transferred| User

  User -->|Bets with Rewards| LotteryProcess
  LotteryProcess --> StakingPlatform
  StakingPlatform -->|Verifies Reward Balance| StakingProvider
  StakingProvider -->|Confirm| StakingPlatform
  StakingPlatform --> LotteryContract(Lottery Contract)
  LotteryContract -->|Bet Confirmed| User

%% Program Interaction Matrix
  StakingPlatform -->|Cross-Program Call| StakingProvider
  StakingPlatform -->|Cross-Program Call| LotteryContract
  LotteryContract -->|Control Flow| StakingPlatform
  StakingProvider -->|Data Transmission| StakingPlatform
  StakingPlatform -->|Data Transmission| LotteryContract

%% Account Management
  User -->|Creates| UserAccount(User Account)
  StakingPlatform -->|Manages| StakingPlatformAccount
  StakingProvider -->|Manages| StakingPoolTokenAccount
  StakingProvider -->|Updates| RewardsTokenAccount
  User -->|Transfers Ownership| MultiSigVault(Multi-Sig Vault)

%% External Integrations
  StakingPlatform -->|Compliance Check| ComplianceProvider(Compliance Provider)
  StakingPlatform -->|Fetch Stake Data| StakingProvider
  User -->|Withdraws to Vault| MultiSigVault
  MultiSigVault -->|Security Check| ComplianceProvider
  ComplianceProvider -->|Approval| MultiSigVault
  MultiSigVault -->|Executes Transfer| User

%% Legend
  classDef userNode fill:#f9f,stroke:#333,stroke-width:2px;
  classDef contractNode fill:#9ff,stroke:#333,stroke-width:2px;
  classDef processNode fill:#ff9,stroke:#333,stroke-width:2px;

  class User,UserAccount,MultiSigVault userNode;
  class StakingPlatform,StakingProvider,LotteryContract,ComplianceProvider,Oracle contractNode;
  class DepositProcess,StakingProcess,RewardClaiming,LotteryProcess processNode;
```
