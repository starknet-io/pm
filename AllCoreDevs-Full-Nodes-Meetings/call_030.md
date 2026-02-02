# Starknet All Core Devs Meeting #30
## Meeting Details

- **Date & Time:** Thursday, July 3, 2025, 11:00-11:30 AM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/live/DDiBTVZeZAk
- **Agenda:** https://github.com/starknet-io/pm/issues/20
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Executive Summary

This meeting provided critical updates on the v0.14.0 testnet deployment timeline and client readiness. Key highlights included:
- Testnet v0.14.0 deployment delayed to July 9th (from June 30th) due to necessary bug fixes and additional testing
- RPC 0.9 RC2 published, enabling WebSocket RPC specification discussions
- Pathfinder v0.17 released with JSON-RPC 0.9, experimental pruned mode, and Cairo Native integration
- Juno Tendermint MVP nearing completion (mid-to-late next week target)
- Kasar Labs resolved Cairo Native and Blockifier issues, WebSocket methods merged
- WebSocket API breaking change requires explicit opt-in for staking validators
- Juno-Malachite interoperability testing meeting scheduled for July 7th

## Meeting Notes

The meeting began with [Aayush Giri](https://github.com/Giri-Aayush) welcoming participants and noting the significant milestone approaching with v0.14.0 testnet deployment. He mentioned that [Leonardo Lerer](https://github.com/leo-starkware) was unable to join but had provided detailed async updates to be shared throughout the call.

### 1. Starknet v0.14.0 Testnet Deployment Update

Aayush shared Leonardo's updates regarding testnet deployment status. Testnet v0.14.0 is now scheduled for Wednesday, July 9th, representing a brief delay from the original June 30th timeline. This delay is attributed to necessary bug fixes and an additional testing phase to ensure deployment stability.

The RPC 0.9 RC2 tag will be published on July 3rd, marking continued progress on RPC implementation. With nodes having successfully published testnet releases, WebSocket RPC discussions can now commence. This development opens new coordination opportunities for specification refinements and implementation strategies across client teams.

Aayush invited teams to share any initial thoughts on coordination requirements moving forward, but no immediate questions were raised from participants.

**Key Discussion Points:**
- Testnet deployment pushed to July 9th from June 30th
- Delay due to bug fixes and extended testing requirements
- RPC 0.9 RC2 tag published July 3rd
- WebSocket RPC specification discussions now unblocked

**Decisions Made:**
- Testnet v0.14.0 deployment confirmed for July 9th, 2025

---

### 2. Client Team Progress Updates

**Pathfinder** ([Krisztian Kovacs](https://github.com/kkovaacs)):

Krisztian reported that Pathfinder v0.17 was released on Monday, July 1st. This release adds support for JSON-RPC 0.9, Starknet v0.14.0, experimental pruned mode, Cairo Native integration, and several other features detailed in the release notes.

The team believes their JSON-RPC 0.9 implementation is complete except for subscription and pre-confirmed block related changes, which remain in progress. A couple of small bugs are currently being fixed, with one affecting pruned mode configuration regarding whether the feature can be enabled or disabled.

Krisztian emphasized that pruned mode is considered an experimental feature. The team has not received extensive feedback yet, apart from some users accidentally enabling pruned mode. A significant breaking change in this Pathfinder release is that the WebSocket API is now opt-in, meaning users cannot use subscription methods unless they explicitly enable WebSocket support. This particular change caused issues for some users, especially stakers, because the validator attestation tool requires subscriptions to be working. However, the solution simply requires explicitly enabling WebSocket support in the new Pathfinder version configuration, after which it works as intended.

The team continues working on WebSocket-related functionality, which Krisztian noted could be discussed later in the call or asynchronously.

When asked about overall testnet readiness, Krisztian confirmed that testnet should be fine. The only complication with this release involves a fairly large database migration taking roughly one hour on mainnet. On testnet the migration completes in just a few minutes, so it should not present issues. For mainnet, the migration constitutes considerably larger downtime during node upgrades. Therefore, the team is currently not recommending mainnet users upgrade immediately. As they approach the mainnet Starknet upgrade date, they will release updated snapshots that can be used instead of running the migration manually.

**Juno** (Rian Hughes, reporting for [Rodrigo Pino](https://github.com/rodrodros)):

Rian provided updates on behalf of Rodrigo, who could not attend. For Tendermint implementation, the team basically has the MVP ready with one or two small issues currently being worked on. Realistically, Rian expects the MVP will be ready mid-to-late next week, and the team is quite confident in hitting that timeline.

Regarding interoperability testing with Malachite, Juno has set up a meeting with the Informal Systems team for Monday, July 7th. This meeting will push forward discussions and enable Juno and Malachite to work together on consensus integration.

Rian also mentioned having updates on RPC and WebSocket implementation but deferred those to the dedicated WebSocket RPC specification agenda item later in the call.

**Kasar Labs** ([Trantorian](https://github.com/Trantorian1)):

Trantorian reported that WebSocket methods have been merged as mentioned in the previous call. The Cairo Native and Blockifier v0.14.0 issues that were causing problems have been resolved and merged into main. Madara currently supports the latest version of Cairo Native and the latest version of Blockifier.

Regarding v0.14.0 sync support, the team has not started this work yet. They are currently refining updates in their sync pipeline regarding L1 messages. Most of the team is out of office at ETHCC and will resume development properly next week.

Trantorian noted that the agenda mentioned new chain features. These remain the same as previously discussed—block time, block update interval, and block size configuration—with no work done on them in the meantime. The team plans to keep these features relatively unchanged.

**Key Discussion Points:**
- Pathfinder: v0.17 released with JSON-RPC 0.9, pruned mode (experimental), Cairo Native, and WebSocket opt-in breaking change
- Pathfinder: 1-hour database migration on mainnet requires careful upgrade planning
- Pathfinder: WebSocket opt-in requirement affects staking validators
- Juno: Tendermint MVP completion mid-to-late next week (July 10-11 approximately)
- Juno: Malachite interop meeting scheduled for July 7th
- Kasar Labs: WebSocket methods merged, Cairo Native and Blockifier v0.14.0 issues resolved
- Kasar Labs: Team at ETHCC, v0.14.0 sync support to resume next week

**Action Items:**
- [Krisztian Kovacs](https://github.com/kkovaacs): Release updated mainnet snapshots before mainnet upgrade (Target: Before July 28, 2025)
- [Krisztian Kovacs](https://github.com/kkovaacs): Complete subscription and pre-confirmed block implementation (Ongoing)
- Rian Hughes / [Rodrigo Pino](https://github.com/rodrodros): Complete Tendermint MVP (Target: Mid-to-late next week, ~July 10-11, 2025)
- Rian Hughes / [Rodrigo Pino](https://github.com/rodrodros): Attend Juno-Malachite interop meeting (Target: July 7, 2025)
- [Trantorian](https://github.com/Trantorian1): Begin v0.14.0 sync support implementation (Target: Week of July 7, 2025)

**Blockers Identified:**
- Pathfinder: 1-hour mainnet database migration creates upgrade complexity
- Kasar Labs: Team availability reduced due to ETHCC attendance

---

### 3. Consensus Implementation & Interoperability Testing

Aayush asked Rodrigo and Adi about the Juno-Malachite interoperability testing plans established in Call #29 for early July. Since the timeline had arrived, he inquired whether preparations were complete for this testing phase.

Rian responded that beyond the Monday meeting he mentioned earlier, there was nothing else to add from Juno's perspective. He confirmed sending the meeting invite for next Monday.

[Adi Seredinschi](https://github.com/adizere) provided an update on Malachite development. He noted there is not much to report because both core engineers were on vacation for more than seven days, so progress has been slower with development and testing. However, the Monday meeting with Juno is confirmed and will move discussions forward.

**Key Discussion Points:**
- Juno-Malachite interoperability meeting scheduled for Monday, July 7th, 2025
- Malachite development slowed due to core engineers on vacation
- Consensus testing preparations on track despite vacation delays

**Action Items:**
- [Adi Seredinschi](https://github.com/adizere) & Rian Hughes / [Rodrigo Pino](https://github.com/rodrodros): Conduct Juno-Malachite interoperability planning meeting (Target: July 7, 2025)

---

### 4. WebSocket RPC Specification & Implementation Coordination

Aayush provided Leonardo's update regarding WebSocket RPC discussions. Substantive discussions regarding WebSocket RPC implementations are ongoing. The team would like to facilitate discussions on required specification changes to support WebSocket functionality, implementation approaches, and coordination strategies across client teams. Technical standards and compatibility considerations should be established, along with timeline expectations for feature completion and deployment.

Aayush invited Rian to provide WebSocket updates from Juno's perspective.

Rian reported that Juno released version 0.15 RC with full support for RPC version 0.9. The team suggests waiting until there is a full release before clients update their mainnet nodes, but testnet nodes should be updated at the moment. Regarding anything more specific about WebSocket implementation details, Rian noted he does not have oversight into those specifics but confirmed that RPC version 0.9 is supported.

**Key Discussion Points:**
- WebSocket RPC specification discussions now enabled following testnet releases
- Juno v0.15 RC released with full RPC 0.9 support
- Recommendation to wait for full release before mainnet node upgrades
- Testnet nodes should upgrade immediately

**Action Items:**
- All client teams: Continue WebSocket RPC specification discussions asynchronously (Ongoing)
- All client teams: Coordinate on WebSocket implementation approaches and technical standards (Ongoing)

---

### 5. Mainnet v0.14.0 Timeline Assessment

Aayush summarized the call's discussions and addressed mainnet timeline planning. With testnet deployment now scheduled for July 9th, approximately 19 days remain until the planned July 28th mainnet deployment. He invited thoughts on whether this timeline provides an adequate testing and validation period and asked about key milestones and success criteria to monitor during this phase.

No participants raised concerns or questions about the timeline.

**Key Discussion Points:**
- 19 days between testnet (July 9th) and planned mainnet (July 28th) deployment
- No timeline concerns raised by client teams
- Success criteria and monitoring to be determined post-testnet deployment

---

### 6. AOB (Any Other Business)

Before concluding, Aayush asked whether any additional topics or coordination needs required immediate attention.

[Shahak Shama](https://github.com/ShahakShama) raised an update regarding consensus peer-to-peer specifications. He pointed out a small update to the consensus P2P specs that allows the proposal to send transactions before executing them. This is accomplished by marking the end of the proposal after sending it—essentially truncating it if they discover they attempted to execute too many transactions and want to remove the last few.

When asked where this information could be referenced, Shahak explained that they have sent a pull request changing this behavior. He mentioned they will hopefully soon write a README corresponding to this change, which will provide more detailed information.

Aayush thanked Shahak and confirmed he would look for that documentation.

No other topics were raised by participants.

**Key Discussion Points:**
- Consensus P2P spec update: proposals can send transactions before execution
- Proposal truncation mechanism enables removing excess transactions
- PR submitted with README documentation to follow

**Action Items:**
- [Shahak Shama](https://github.com/ShahakShama): Complete README documentation for consensus P2P spec changes (Target: Soon after July 3, 2025)

---

Aayush concluded the meeting by noting the next call is scheduled for July 17th, 2025. By then, testnet will be operational and the team can assess real-world performance and gather feedback for discussion. He thanked all participants for their continued dedication to the Starknet ecosystem and expressed optimism for a successful testnet deployment.

---

## Key Decisions Summary

1. **Testnet Deployment Date**
   - July 9th, 2025 confirmed (delayed from June 30th)
   - Delay attributed to necessary bug fixes and additional testing phase

2. **Pathfinder Mainnet Upgrade Strategy**
   - Do not recommend immediate mainnet upgrade due to 1-hour database migration
   - Updated snapshots will be provided closer to mainnet upgrade date

3. **WebSocket Breaking Change**
   - Pathfinder WebSocket API now opt-in (explicit configuration required)
   - Affects staking validators using attestation tools requiring subscriptions

4. **Juno-Malachite Interoperability Timeline**
   - Planning meeting confirmed for July 7th, 2025
   - Testing to proceed following planning discussions

5. **Consensus P2P Specification Update**
   - Proposals can send transactions before execution with truncation capability
   - Documentation to follow in upcoming README

---

## Action Items Tracker

| Owner | Action | Target Date/Call | Status |
|-------|--------|------------------|--------|
| Krisztian Kovacs | Release updated mainnet snapshots for migration | Before July 28, 2025 | Open |
| Krisztian Kovacs | Complete subscription and pre-confirmed block implementation | Ongoing | In Progress |
| Rian Hughes / Rodrigo Pino | Complete Tendermint MVP | July 10-11, 2025 | In Progress |
| Rian Hughes / Rodrigo Pino | Attend Juno-Malachite interop planning meeting | July 7, 2025 | Open |
| Trantorian | Begin v0.14.0 sync support implementation | Week of July 7, 2025 | Pending |
| Adi Seredinschi | Attend Juno-Malachite interop planning meeting | July 7, 2025 | Open |
| Shahak Shama | Complete README for consensus P2P spec changes | Post-July 3, 2025 | Open |
| All client teams | Continue WebSocket RPC specification discussions | Ongoing | In Progress |
| All client teams | Coordinate WebSocket implementation approaches | Ongoing | In Progress |

---

## Attendees

- **Moderator:** Aayush Giri | Nethermind
- Krisztian Kovacs | Equilibrium
- Rian Hughes | Nethermind
- Thiago Ribeiro | Nethermind
- Trantorian | Kasar Labs
- Adi Seredinschi | Informal Systems
- Shahak Shama | Starkware
- Eitan Moed | Starkware
- Ariel Elperin | Starkware
- Vaclav | Equilibrium
- Chris | Equilibrium
- cchudant

---

## Glossary

- **Pre-confirmation**: Feature in RPC 0.9 enabling faster transaction confirmation status before full block confirmation
- **Pruned Mode**: Database configuration retaining only recent blocks rather than full history, significantly reducing storage requirements; experimental feature in Pathfinder v0.17
- **WebSocket API**: Real-time bidirectional communication protocol enabling subscription-based RPC methods; now opt-in in Pathfinder v0.17
- **Database Migration**: Process of updating database schema and structure during software upgrades; Pathfinder v0.17 migration takes ~1 hour on mainnet
- **Tendermint MVP**: Minimum viable product of Tendermint BFT consensus implementation in Juno client
- **Proposal Truncation**: Consensus mechanism allowing removal of excess transactions from proposals before execution completion
- **RPC 0.9 RC2**: Second release candidate of JSON-RPC specification version 0.9

---

**Next Meeting:** Thursday, July 17, 2025, 11:00 AM UTC

**Note:** These Starknet All Core Devs Calls occur bi-weekly at the same time. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.
