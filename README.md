# Capstone project - TURBIN3 Q1 2025

Capstone project for Turbin 2025 Q1

[LOI](https://docs.google.com/document/d/1RnwgPXAF4HwqqXloj5LX-fdnMdOWr92yRmsojCze8H4/edit?tab=t.0)

## Front End

> [Leptos](https://book.leptos.dev/)
>
> [Awesome Leptos](https://github.com/leptos-rs/awesome-leptos)
>
> [Thaw UI](https://github.com/thaw-ui/thaw)

## Sequence Diagram

```mermaid
sequenceDiagram
  participant User
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

## Mindmap

```mermaid
mindmap
  root((Staking Web3 Site on Solana))
    User
      - Request Staking
      - Stake SOL
      - Receive Rewards
      - Bet with Rewards
    AIAgent
      - Analyze Platforms
      - Recommend Staking Platform
    Staking Platform
      - Stake SOL with Provider
      - Distribute Rewards
      - Verify Rewards for Betting
    Staking Provider
      - Confirm Staking
      - Distribute Staking Rewards
      - Verify Rewards Availability
    Lottery Contract
      - Accept Bet with Rewards
      - Confirm Bet
```
