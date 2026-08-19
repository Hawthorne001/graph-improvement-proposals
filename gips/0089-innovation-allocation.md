| GIP | 0089 |
| :--- | :--- |
| Title | Innovation Allocation |
| Authors | Nick Hansen (Team Lead, The Graph Foundation)<br>Ørjan Auestad (Head of Protocol & Network, The Graph Foundation) |
| Created | 2026-08-19 |
| Stage | Candidate |
| Category | Protocol Logic |
| Discussion |  |
| Depends on | [GIP-0070](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0070.md), [GIP-0076](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0076.md), [GIP-0088](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0088.md) |


## Abstract

This GIP proposes allocating 20% of protocol issuance to The Graph Foundation to accelerate innovation across The Graph ecosystem. Using the Issuance Allocator introduced in [GIP-0076](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0076.md) and deployed via [GIP-0088](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0088.md), 24.146 GRT per block of the total 120.73 GRT per block issuance is allocated to a Foundation-controlled DirectAllocation contract.

## Introduction

The Graph community has shown strong support for upgrading network economics as outlined in [GIP-0070](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0070.md). The Issuance Allocator ([GIP-0076](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0076.md)) created a flexible issuance allocation mechanism that enables governance to configure how issuance is distributed across multiple targets, and [GIP-0088](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0088.md) deployed it with the Rewards Manager as the sole target receiving 100% of issuance.

This proposal introduces an **Innovation Allocation**: a governance-determined amount of GRT allocated to The Graph Foundation to accelerate growth across The Graph ecosystem, while maintaining network stability. 

## Motivation

The Graph has an opportunity to accelerate innovation by strategically directing issuance. How issuance is distributed shapes what gets built. Optimizing it expands the ecosystem and increases the network's value to users.

Directing a portion of issuance to innovation projects accelerates the development of new features, tools, and integrations that expand The Graph ecosystem and compound its utility.

The Innovation Allocation also secures the Foundation's ability to support the ecosystem beyond its original ten-year vesting contract, established at network launch. A dedicated stream of issuance gives the Foundation a sustainable way to continue growing the ecosystem: ensuring long-term continuity for the network, and giving builders and participants confidence today that the Foundation will be there tomorrow.

## Prior Art

For the context of this proposal, see [GIP-0070](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0070.md), which outlines the long-term vision for evolving The Graph protocol economics.

This proposal builds on [GIP-0076](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0076.md), which introduced the Issuance Allocator and the DirectAllocation contract, and [GIP-0088](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0088.md), which deployed the Issuance Allocator and connected it to the Rewards Manager. 

## High-Level Description

This proposal requires no new smart contract code. It deploys a new instance of the existing, [audited](https://github.com/graphprotocol/contracts/tree/687d928b1336d1af594a3b0ed23326db6cb3a03f/packages/issuance/audits) [DirectAllocation contract](https://github.com/graphprotocol/contracts/blob/687d928b1336d1af594a3b0ed23326db6cb3a03f/packages/issuance/contracts/allocate/DirectAllocation.sol) and uses the deployed issuance allocation mechanism:

1. **A DirectAllocation instance for the Innovation Allocation.** A generic DirectAllocation contract ([GIP-0076](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0076.md)) is deployed and configured with The Graph Foundation as the operator. It is an allocator-minting target: the Issuance Allocator mints tokens directly to this contract, from which the Foundation can withdraw them as needed.
2. **A governance-approved allocation of 20% of issuance.** The Issuance Allocator maintains the existing total issuance per block rate (120.73 GRT) and allocates 24.146 GRT per block to the Innovation Allocation. The Rewards Manager's allocation is reduced from 120.73 to 96.584 GRT per block accordingly.

The Innovation Allocation will be directed by The Graph Foundation to support innovation projects:

- A governance-determined amount of GRT will be allocated.
- The Foundation will have discretion over how these tokens are used.

## Detailed Specification

### 1. Innovation Allocation Management

The Innovation Allocation is implemented using the [audited](https://github.com/graphprotocol/contracts/tree/687d928b1336d1af594a3b0ed23326db6cb3a03f/packages/issuance/audits) [DirectAllocation contract](https://github.com/graphprotocol/contracts/blob/687d928b1336d1af594a3b0ed23326db6cb3a03f/packages/issuance/contracts/allocate/DirectAllocation.sol), configured with The Graph Foundation as the operator. The Issuance Allocator mints tokens directly to this contract, and the authorized operator can withdraw and transfer them to individual addresses as needed.

### 2. Allocation Configuration

Governance configures the split in the Issuance Allocator:

| Target | Type | Allocation (GRT per block) | Share |
| :--- | :--- | ---: | ---: |
| Rewards Manager | Self-minting | 96.584 | 80% |
| Innovation DirectAllocation | Allocator-minting | 24.146 | 20% |
| **Total** |  | **120.73** | **100%** |

The total issuance rate does not change; it remains at 120.73 GRT per block. This allocation is expected to be configured before the [GIP-0088](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0088.md) Phase 3 split to the Recurring Agreement Manager. When that split is subsequently activated, 6 GRT per block will be allocated to the Recurring Agreement Manager, reducing the Rewards Manager's share from 96.584 to 90.584 GRT per block.

For reference, there were 2,610,162 mainnet blocks in 2025. At 24.146 GRT per block, 20% of total issuance, the Innovation Allocation corresponds to approximately 63 million GRT per year. 

### 3. Governance and Monitoring

Protocol governance approves the allocation percentage, with regular community reporting and adjustments based on results and feedback:

- The Graph Council approves the initial allocation.
- The use of the Innovation Allocation will be reported to the community annually.
- Adjustments to the allocation percentage will be made based on results and community feedback.

## Implementation

The implementation requires no new smart contract code and no contract upgrades; only a new deployment of the existing [DirectAllocation contract](https://github.com/graphprotocol/contracts/blob/687d928b1336d1af594a3b0ed23326db6cb3a03f/packages/issuance/contracts/allocate/DirectAllocation.sol). The Issuance Allocator, its minter role on GraphToken, and the Rewards Manager integration are all in place following [GIP-0088](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0088.md).

1. **Deploy DirectAllocation instance**: Deploy a DirectAllocation contract with The Graph Foundation multisig as the operator.
2. **Configure allocation**: Governance adds the DirectAllocation as an allocator-minting target and sets the split: Rewards Manager 96.584 GRT per block, Innovation Allocation 24.146 GRT per block.
3. **Verify**: Confirm target allocations sum to 120.73 GRT per block, the Rewards Manager reads its reduced rate from the allocator, and issuance is minted to the DirectAllocation on distribution.

Throughout the implementation, the total issuance rate remains exactly the same.

## Backward Compatibility

This proposal introduces no new smart contract code and makes no changes to existing protocol contracts; it deploys a new instance of the existing DirectAllocation contract. The Rewards Manager continues to operate unchanged, with its issuance rate sourced from the Issuance Allocator as configured in [GIP-0088](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0088.md).

Indexer rewards from issuance decrease proportionally with the reduced Rewards Manager allocation. This is an intentional policy change: governance is directing a portion of issuance to fund ecosystem innovation rather than pure indexing rewards. All existing indexer stakes and delegations continue to earn rewards at the expected rate, scaled by the new allocation percentage.

## Risks and Security Considerations

1. **Smart Contract Risk**: No new smart contract code is required. The DirectAllocation contract is the audited implementation from [GIP-0076](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0076.md), deployed as a new instance. Risk is mitigated by reusing existing, audited code and the deployed allocation mechanism.
2. **Economic Impact**: Directing a portion of issuance reduces indexing rewards for both indexers and their delegators. This allocation is smaller than the share of issuance that currently does not reach value-providing participants, and the timing of this proposal is deliberately aligned with three initiatives that improve the economic efficiency of the network:
    - **Rewards Eligibility Oracle** ([GIP-0079](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0079.md), integrated via [GIP-0086](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0086.md)): lands before this GIP. Indexers that do not meet minimum service standards will no longer receive rewards. Based on 2025 numbers, 15.2% of indexing rewards went to indexers providing effectively no value to the network, along with their delegators, and a further 11.8% to indexers with very low uptime or supporting fewer than 20 query-producing subgraphs. Eligibility criteria will be progressively strengthened to reclaim this issuance.
    - **Liquid staking initiative**: phase one launches before this GIP, strengthening the economic efficiency of delegation and supporting indexers that provide meaningful value to the network.
    - **Indexing agreements (DIPS)** ([GIP-0087](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0087.md), [GIP-0088](https://github.com/graphprotocol/graph-improvement-proposals/blob/main/gips/0088.md)): currently under testing and expected to go live around the same time or shortly after this GIP, enabling indexers to earn rewards by syncing specific subgraphs under on-chain agreements, tying rewards more directly to tangible network contributions.
3. **Governance Risk**: The governance-controlled allocation introduces centralization risk. This is mitigated by:
    - Transparent governance processes.
    - The Graph Council approves the allocation and maintains continued oversight of how the funds are used.

## Copyright Waiver

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
