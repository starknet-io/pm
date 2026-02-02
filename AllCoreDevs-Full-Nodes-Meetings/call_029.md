# Starknet All Core Devs Meeting #29
## Meeting Details

- **Date & Time:** Thursday, June 19, 2025, 11:00-11:30 AM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/live/vcX2UO9zWyk
- **Agenda:** https://github.com/starknet-io/pm/issues/19
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Executive Summary

This meeting occurred just two days after the Staking v2 launch on June 17th and focused on critical v0.14.0 deployment preparations. Key highlights included:
- Potential one-week delay for testnet deployment (from June 30th) due to partner readiness concerns
- RPC 0.9 RC1 published with pre-confirmation features
- Successful Staking v2 launch with 53 active validators covering majority of stake
- All client teams progressing on v0.14.0 integration with varying completion timelines
- Juno-Malachite interoperability testing planned for early July

## Meeting Notes

The meeting began with [Aayush Giri](https://github.com/Giri-Aayush) welcoming participants and emphasizing the packed agenda, noting this was a critical checkpoint just two days after the Staking v2 launch and approximately five weeks before the planned mainnet v0.14.0 deployment.

### 1. Starknet v0.14.0 Testnet Deployment Status

[Leonardo Lerer](https://github.com/leo-starkware) provided comprehensive updates on v0.14.0 testnet deployment preparations. He reported that the June 30th date, planned several weeks prior, may face up to a one-week delay at worst. The primary concern centers on external partner readiness, as not all necessary partners will be able to support the required changes on time. For now, the team maintains the original June 30th target date while planning to reassess as the deployment approaches.

Leonardo confirmed that v0.14.0 is currently on integration and was upgraded early in the week of June 16th with pre-confirmation features required for RPC 0.9. Client teams have begun integrating these capabilities into their implementations. The team published the first release candidate of RPC 0.9 earlier, and on June 19th published RC1 with several merged pull requests addressing initial feedback.

Ongoing discussions continue regarding key configuration parameters for mainnet deployment, specifically maximum transaction size and minimum fee price for L2 gas in the fee market. Leonardo expressed optimism that the community would converge on these parameters relatively quickly.

When asked about partner concerns, Leonardo clarified the issues relate primarily to the deprecation of all non-V3 transaction versions. Several partners plan to complete their migration during the testnet phase, which is acceptable. However, a couple of partners requested readiness from testnet launch, requiring them to support RPC 0.8 in their write APIs and deprecate older transaction versions.

Regarding mainnet timeline confidence, Leonardo acknowledged the complexity of the question. While he expects the majority of partners to be ready for transaction version deprecation, the critical path involves ensuring SDKs support RPC 0.9, followed by sufficient time for applications consuming these SDKs to adapt their workflows. He noted that supporting RPC 0.9 for applications only caring about pre-confirmation (not the faster finality status) should require relatively small changes, but coordinating many moving pieces simultaneously makes definitive timeline commitments challenging.

**Key Discussion Points:**
- RPC 0.9 RC1 published June 19th with pre-confirmation support
- Integration environment upgraded with pre-confirmation features week of June 16th
- Maximum transaction size and minimum L2 gas fee price still under community discussion
- Partner migration primarily blocked on transaction V3 and RPC 0.8 write API support
- SDK support for RPC 0.9 followed by application migration represents critical path

**Decisions Made:**
- June 30th testnet date maintained as target with potential one-week delay if needed
- Decision point to be reassessed closer to deployment date based on partner readiness

**Blockers Identified:**
- External partner readiness for transaction version deprecation
- SDK support timeline for RPC 0.9
- Application migration timeline after SDK updates become available

---

### 2. Staking v2 Launch Results

Aayush reported that [Natan Granit](https://github.com/natangranit) could not attend the call but provided comprehensive launch statistics. The June 17th upgrade proceeded smoothly with 10 decentralized applications supporting the transition. The network achieved 53 active validators covering the majority of staked tokens, with participation trackable through Voyager and Endure explorers. The team continues working on Binance integration and reaching out to long-tail validators to expand participation.

Natan expressed special gratitude to [Krisztian Kovacs](https://github.com/kkovaacs) and [Rodrigo Pino](https://github.com/rodrodros) for their personal support during the transition. He also identified a need for documentation improvements in the Starkware validator guide, particularly in the attestation section.

Aayush invited Krisztian and Rodrigo to discuss common issues and troubleshooting experiences during the launch.

[Rodrigo Pino](https://github.com/rodrodros) identified the primary issue encountered: validators using two-factor authentication on their accounts experienced complications when validating and performing attestation transactions. He noted that some validators had valid private keys but incorrect operational addresses configured. Overall, Rodrigo characterized the launch as proceeding mostly smoothly.

[Krisztian Kovacs](https://github.com/kkovaacs) reported similar experiences, noting most issues stemmed from misconfiguration rather than fundamental technical problems. Common misconfigurations included using incorrect private keys, employing outdated Braavos accounts fundamentally incompatible with JSON-RPC 0.8, enabling two-factor authentication, or activating various security features in account contracts used for operational accounts. These account contract features should not have been enabled for validator operational accounts.

Krisztian also reported technical issues on the Pathfinder side where some validators attempted to use HTTPS and TLS when connecting to nodes. This configuration did not work initially but was resolved in a subsequent release. He concluded that given the large scope and complexity of the Staking v2 upgrade process, the transition proceeded fairly smoothly.

**Key Discussion Points:**
- 53 active validators successfully participating post-launch
- 10 dApps supporting the transition
- Most issues related to account configuration rather than fundamental technical problems
- Two-factor authentication and security features in operational accounts caused complications
- TLS/HTTPS connectivity issues in Pathfinder resolved with subsequent release

**Lessons Learned:**
- Operational accounts for validators should not use enhanced security features like 2FA
- Documentation needs improvement in attestation sections of validator guides
- Using outdated account types (e.g., old Braavos accounts) causes JSON-RPC 0.8 incompatibility

---

### 3. Client Team v0.14.0 & Staking v2 Support Updates

**Pathfinder** ([Krisztian Kovacs](https://github.com/kkovaacs)):

Krisztian provided updates across three areas. Regarding Staking v2 attestation performance, he reported waiting for feedback from several validators claiming failing attestation transaction submissions. The team wants to understand exactly what is occurring in these cases. Apart from these specific reports, no complaints have been received regarding attestation performance.

For Starknet v0.14.0 support including JSON-RPC 0.9 and new features like pre-confirmed blocks, Pathfinder has just merged a basic implementation of pre-confirmed block polling. The node now updates its state with data available from the feeder gateway for pre-confirmed blocks. JSON-RPC 0.9 implementation has basically just started, with the team eagerly awaiting the new specification release containing significant changes from an implementation perspective compared to the previous version. The team plans to prioritize implementing differences between RPC 0.9 and 0.8 as top priority.

For pruned mode implementation, Pathfinder successfully tested syncing mainnet using a configuration retaining only the last 2,000 blocks. Results proved promising, with the complete database after an up-to-date mainnet sync (as of the week of June 12th) consuming approximately 50 GB of storage—significantly less data than a full state database. This feature will be rolled out as experimental in the next Pathfinder release, though not yet recommended for production use. No showstopper problems are currently known.

**Juno** ([Rodrigo Pino](https://github.com/rodrodros)):

Rodrigo reported that v0.14.0 development continues with the team completing implementation of RPC 0.9. A parallel branch exists for this work with experimental releases intended strictly for internal use, not recommended for external adoption. The team is testing these builds internally while implementing pre-confirmed blocks and related features.

Regarding Tendermint implementation, everything progresses well with the team remaining close to their end-of-June goals. Some final connections need completion, but they expect an MVP ready by end of June or first week of July. The team allowed an extra week of flexibility for unforeseen issues. The MVP will achieve consensus under benign conditions, with nodes successfully communicating with each other. Rodrigo confirmed coordination with Adi Seredinschi for conducting interoperability tests, acknowledging these will likely uncover issues requiring resolution.

For Staking v2, Rodrigo confirmed everything proceeded smoothly. Similar to Krisztian's experience, all issues encountered related primarily to misconfiguration, with validators experiencing successful operation after resolving configuration problems.

**Kasar Labs** ([Trantorian](https://github.com/Trantorian1)):

Trantorian reported steady forward progress on v0.14.0 support. For RPC 0.8.1 method support, pull requests are in review for every outstanding method, passing continuous integration checks and having completed several rounds of internal review. The team expects to merge these into main within the next few days.

Regarding Blockifier updates, the team has encountered technical difficulties. Trantorian mentioned that Sharpa was experiencing issues related to Blockifier integration but was not present on the call to provide detailed technical context. The team is currently fixing bugs on this front, placing them behind schedule for the v0.14.0 update.

Implementation of new transaction statuses for RPC and pre-confirmed block synchronization will begin the week of June 23rd. In more general updates, the team has pushed various fixes to the Madara codebase addressing L1 messaging and new admin capabilities. These admin capabilities allow operators running Madara nodes to add transactions with custom priority, bypassing block validation and mempool to enable permissioned state production. This aims to make Madara increasingly flexible for custom chains, devnets, and mock test environments. The team also encountered and subsequently fixed bugs in their Bloom filter implementation.

When asked about Cairo Native dependencies and potential blockers, Trantorian clarified that issues with Cairo Native relate to the v0.14.0 Blockifier upgrade. Sharpa has been debugging these issues over recent weeks but was unavailable to provide technical details. Trantorian committed to forwarding any answers via the Slack channel.

**Key Discussion Points:**
- Pathfinder: Pre-confirmed block polling merged, RPC 0.9 implementation starting, pruned mode shows 50GB database size for full mainnet sync
- Juno: RPC 0.9 implementation complete in experimental branch, Tendermint MVP targeting end of June/early July
- Kasar Labs: RPC 0.8.1 methods nearly merged, Blockifier integration facing technical difficulties, admin capabilities added for permissioned state production

**Action Items:**
- [Krisztian Kovacs](https://github.com/kkovaacs): Investigate reported attestation transaction submission failures (Ongoing)
- [Krisztian Kovacs](https://github.com/kkovaacs): Complete RPC 0.9 implementation prioritizing specification differences (Target: Post-RC2 release)
- [Rodrigo Pino](https://github.com/rodrodros): Complete Tendermint MVP (Target: June 30 - July 7, 2025)
- [Trantorian](https://github.com/Trantorian1): Merge RPC 0.8.1 method implementations (Target: June 20-23, 2025)
- [Trantorian](https://github.com/Trantorian1): Begin new transaction status and pre-confirmed block sync implementation (Target: Week of June 23, 2025)
- Sharpa (Kasar Labs): Resolve Blockifier v0.14.0 Cairo Native integration issues (Ongoing)

**Blockers Identified:**
- Kasar Labs: Blockifier v0.14.0 integration blocked by Cairo Native technical issues
- All teams: RPC 0.9 specification finalization needed for complete implementation

---

### 4. Consensus Implementation Progress

Aayush reported that [Adi Seredinschi](https://github.com/adizere) could not join the call but provided updates on Malachite consensus development. Interoperability tests have been completed as mentioned in Call #28. Adi confirmed with Rodrigo preparation to start Juno-Malachite interoperability testing in early July.

From an engineering perspective, the Malachite team is preparing to release v0.3.0 with numerous small fixes, peer scoring support, and batching features. Ongoing conversations continue with [Shahak Shama](https://github.com/ShahakShama) and Matan regarding L2 reorganization design and adjusting the fault model for the Apollo sequencer.

Aayush asked Rodrigo to confirm the early July timeline for Juno-Malachite interoperability testing and coordinate any necessary preparation steps.

[Rodrigo Pino](https://github.com/rodrodros) confirmed the testing approach will proceed in phases. First, Juno must achieve consensus with itself under internal testing conditions. Simultaneously, the team will begin the integration process with Malachite to understand how to integrate into their testing framework. Rodrigo estimated preliminary results would be available in the second week of July.

**Key Discussion Points:**
- Malachite interoperability tests completed per Call #28
- Malachite v0.3.0 pending release with peer scoring, batching features, and bug fixes
- L2 reorg design discussions ongoing with Starkware P2P team
- Juno internal consensus testing precedes Malachite interoperability testing

**Action Items:**
- [Adi Seredinschi](https://github.com/adizere): Release Malachite v0.3.0 (Target: Late June 2025)
- [Rodrigo Pino](https://github.com/rodrodros): Complete internal Juno consensus testing (Target: End of June 2025)
- [Rodrigo Pino](https://github.com/rodrodros) & [Adi Seredinschi](https://github.com/adizere): Begin Juno-Malachite interoperability testing (Target: Early July 2025)
- [Rodrigo Pino](https://github.com/rodrodros) & [Adi Seredinschi](https://github.com/adizere): Produce preliminary interoperability test results (Target: Second week of July 2025)

---

### 5. Mainnet v0.14.0 Planning

Aayush noted this topic had been largely covered during the testnet deployment discussion but invited Leonardo to discuss checklists for mainnet readiness and coordination with RPC providers.

[Leonardo Lerer](https://github.com/leo-starkware) confirmed that coordination with RPC providers follows the standard process: nodes release new versions, followed by requests for providers to upgrade their infrastructure. Nothing differs in this upgrade cycle regarding provider coordination.

What distinguishes this upgrade is the extensive months-long outreach to every integrated partner on Starknet, communicating that only transaction V3 with RPC 0.8 will be supported in v0.14.0 and requiring backend upgrades. This partner communication has been ongoing for several months.

Leonardo identified the primary checklist item as ensuring the vast majority of partners achieve readiness with V3 transactions and RPC 0.8 support—essentially a mandatory requirement. Beyond partner readiness, the critical path involves ensuring tooling availability with sufficient time for ecosystem participants to adapt. However, Leonardo characterized this as the standard challenge for every upgrade cycle.

**Key Discussion Points:**
- RPC provider coordination follows standard upgrade process
- Multi-month partner outreach for transaction V3 and RPC 0.8 migration
- Vast majority partner readiness classified as mandatory requirement
- Tool availability and adaptation time represents standard upgrade challenge

---

### 6. AOB (Any Other Business)

Aayush opened the floor for additional topics or discussions. With no items raised and six minutes remaining in the scheduled time, he thanked participants for joining. He expressed anticipation for observing testnet deployment progress and noted the next call (Call #30, July 3, 2025) would provide opportunities for post-testnet deployment feedback and status updates.

---

## Key Decisions Summary

1. **Testnet Deployment Timeline**
   - June 30th date maintained as target with potential one-week delay
   - Decision point for delay to be made closer to deployment based on partner readiness assessment

2. **RPC 0.9 Development Priority**
   - Client teams to prioritize RPC 0.9 implementation upon specification finalization
   - Focus on implementing differences from RPC 0.8 rather than full reimplementation

3. **Staking v2 Launch Assessment**
   - Launch characterized as successful despite configuration-related issues
   - Documentation improvements needed for validator guides, particularly attestation sections

4. **Consensus Testing Phasing**
   - Juno internal testing precedes Malachite interoperability testing
   - Preliminary cross-client results targeted for second week of July

---

## Action Items Tracker

| Owner | Action | Target Date/Call | Status |
|-------|--------|------------------|--------|
| Leonardo Lerer | Assess partner readiness for testnet deployment decision | June 26-27, 2025 | Open |
| Leonardo Lerer | Finalize RPC 0.9 specification for client implementation | June 20-23, 2025 | In Progress |
| Krisztian Kovacs | Investigate reported attestation transaction failures | Ongoing | Open |
| Krisztian Kovacs | Release Pathfinder with experimental pruned mode feature | Late June 2025 | Open |
| Krisztian Kovacs | Implement RPC 0.9 specification differences | Post-RC2 release | Pending |
| Rodrigo Pino | Complete Tendermint MVP implementation | June 30 - July 7, 2025 | In Progress |
| Rodrigo Pino | Complete internal Juno consensus testing | End of June 2025 | In Progress |
| Rodrigo Pino | Begin Juno-Malachite interoperability testing | Early July 2025 | Pending |
| Trantorian | Merge RPC 0.8.1 method implementations to main | June 20-23, 2025 | In Progress |
| Trantorian | Begin new transaction status and pre-confirmed block sync | Week of June 23, 2025 | Pending |
| Sharpa (Kasar Labs) | Resolve Blockifier v0.14.0 Cairo Native integration issues | Ongoing | Blocked |
| Adi Seredinschi | Release Malachite v0.3.0 | Late June 2025 | In Progress |
| Natan Granit | Improve validator documentation (attestation sections) | Post-launch | Open |
| All client teams | Complete RPC 0.9 integration for testnet deployment | Target testnet date | In Progress |

---

## Attendees

- **Moderator:** Aayush Giri | Nethermind
- Leonardo Lerer | Starkware
- Krisztian Kovacs | Equilibrium
- Rodrigo Pino | Nethermind
- Trantorian | Kasar Labs
- Vaclav | Equilibrium
- Adi Seredinschi | Informal Systems
- Shahak Shama | Starkware
- Eitan Moed | Starkware
- JB | Kasar Labs
- Chris | Equilibrium
- cchudant
- Denisa Diaconescu

---

## Glossary

- **Pre-confirmation**: Feature in RPC 0.9 enabling faster transaction confirmation status before full block confirmation
- **Transaction V3**: Latest transaction version required in v0.14.0, used with RPC 0.8 write APIs
- **Pruned Mode**: Database configuration retaining only recent blocks (e.g., last 2,000) rather than full history, significantly reducing storage requirements
- **MVP (Minimum Viable Product)**: Initial implementation demonstrating core functionality under benign conditions
- **Interoperability Testing**: Cross-client testing to ensure different consensus implementations can communicate and reach agreement
- **L2 Gas**: Resource model for execution-related costs on Starknet Layer 2

---

**Next Meeting:** Thursday, July 3, 2025, 11:00 AM UTC

**Note:** These Starknet All Core Devs Calls occur bi-weekly at the same time. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.
