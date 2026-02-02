# Starknet All Core Devs Meeting #44
## Meeting Details

- **Date & Time:** Thursday, January 29, 2026, 12:00-12:30 PM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/watch?v=8_Li3FTuhT8
- **Agenda:** https://github.com/starknet-io/pm/issues/34
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Screenshot

![Call #44 Attendees](images/call_044_attendees.png)

## Executive Summary

This meeting returned to the regular 30-minute format and covered network stability, decentralization progress, and significant client team developments. Key outcomes included:

- **Network stable since reorg** - No incidents since January 13; postmortem published on starknet.io
- **Decentralized validation preparations underway** - Starkware engaging stakers on expectations; working on observability, alerting, key management, and sync time considerations
- **New RPC crates published (dev versions)** - Blockifier and Starknet API crates released for testing; JSON RPC 0.10.1 releasing before v0.14.2 compatibility
- **Pathfinder pre-release coming** - Will include client-side proofs support for testing; database migration required
- **Juno DB reduction release imminent** - ~700 GB → ~360 GB (nearly 50% reduction); opt-in in v0.15.18, default in v0.16
- **Madara synced Sepolia to tip** - Cairo Native showing 2-3x performance gains; working on RPC 0.10.1 with starknet-rust contributions
- **Malachite team absent third consecutive call** - Aayush to make announcement next call

## Meeting Notes

The meeting opened with [Aayush Giri](https://github.com/Giri-Aayush) welcoming everyone to Starknet All Core Devs Call #44. He covered housekeeping items and reminded everyone of the 12:00 PM UTC time slot continuing through end of March. The call returned to the regular 30-minute format with five main agenda items.

### 1. Network Stability and Decentralized Validation Update

Aayush noted that the reorg incident postmortem had been published on starknet.io and asked for an update on network stability.

[Eitan Moed](https://github.com/eitanm-starkware) reported there have been no incidents since the reorg—a good sign. Network stability has been excellent.

**Decentralized Validation Progress:**

Eitan shared that decentralized validation is taking a "big jump" this year. Starkware is currently engaging with different stakers to understand their expectations for running a sequencer node as part of Starknet.

**Key Considerations Being Addressed:**

1. **Observability and Alert Systems** - When building a decentralized system, operators need proper tools to observe their nodes locally. This was not a priority for centralized operations.

2. **Key Management Solutions** - Evaluating available key management solutions to determine relevance for the Starknet use case, or whether alternative approaches are needed.

3. **Sync Time** - Analyzing how long it will take for operators to sync with the network, including both initial syncs and upgrade syncs.

Eitan noted these are the main areas being addressed to meet validator expectations. There is no specific update on Apollo packaging progress—all the mentioned work relates to creating that comprehensive validator package.

**New RPC Crates Published:**

Eitan announced that new crates were published earlier that day:
- **Blockifier crate**
- **Starknet API crate**

These are **developer versions for testing purposes only**, not intended for stable node releases. The crates are work-in-progress, particularly the Starknet API crate. Stable versions will be published closer to the next network release.

**JSON RPC 0.10.1 Timeline:**

Aayush noted a Slack discussion about JSON RPC 0.10.1 releasing before v0.14.2 compatibility. Eitan confirmed this is a classic case where client releases are compatible with a future network version that has not yet been upgraded. This is unavoidable when upgrading nodes before the network.

### 2. Propeller Implementation Coordination (Skipped)

Andrew was not available for this call. Eitan suggested Andrew would be more qualified to provide detailed answers on Propeller implementation. If there is demand, Andrew can return in two weeks.

### 3. Client Team Updates

Aayush transitioned to client updates, requesting approximately 5 minutes per team.

#### **Pathfinder** ([Krisztian Kovacs](https://github.com/kkovaacs)):

Krisztian reported that v0.21.5 remains the latest release and is working well with no reports of malfunction or sync stalls.

**JSON RPC 0.10.1 Development:**

With today's Blockifier crate release, the team will likely cut a **pre-release** of Pathfinder with client-side proofs support for testing. This will not be a full-fledged release and is not recommended for general consumption or running as full nodes.

**Important Caveats:**
- Database migration will be required
- Users running the pre-release cannot revert to earlier Pathfinder versions without a database backup
- This will be clearly noted in release notes and versioned as a pre-release

**Consensus Implementation Progress:**

The team is improving validation mode by switching to **concurrent execution** for transaction batches received. They are also studying Starkware's Propeller code to understand implementation details.

**Propeller Review:**

When asked about feedback on Andrew's Propeller presentation, Krisztian noted they have not deeply reviewed it yet. They are working on understanding both the code and the concept, including how it differs from and is similar to **Solana's Turbine**.

#### **Juno** ([Rodrigo Pino](https://github.com/rodiazet)):

**Database Reduction Release:**

Rodrigo provided a comprehensive update on the database optimization work:

- A preparation release was made earlier in the week (migrations, bug fixes from December)
- The main database reduction release is coming **today or tomorrow**
- Will be **opt-in initially** as part of v0.15.18 (not a breaking change)
- Specifically targeted at validators who can migrate at their own pace
- Comes with heavy migration and updated DB compression

**Performance Improvements:**

| Metric | Before | After |
|--------|--------|-------|
| Database Size | ~700 GB | ~360 GB |
| Improvement | - | ~50% reduction |

Additional benefits:
- Faster sync times
- Faster RPC response times
- New exposed parameters for users with more hardware to scale performance

**Release Roadmap:**

- **v0.15.18**: Database reduction as opt-in flag
- **v0.16**: Migration becomes default (expected mid-February)
- Documentation explaining all parameters coming with release

**Propeller Planning:**

Rodrigo noted they have not dived deep into Propeller yet due to focus on database optimization. After January, focus will shift to Propeller and consensus implementation, including a Go reimplementation for optimal performance.

#### **Karnot/Madara** ([Mohit](https://github.com/mohit-karnot)):

Mohit provided updates on behalf of the Karnot team.

**SNOS v0.14.1 Status:**

Still pending Dojo team review and merge. The issue is that v0.14.1 introduced two proofs which are not compatible with L3s yet—there is no verifier on Starknet for this. Dojo team has not been able to test thoroughly. Karnot is facing the same issue for their L3 networks and needs to move to v0.14.2.

**Downstream Pipeline Progress:**

Major focus has been improving the downstream pipeline (SNOS aggregator, batching, encrypted DA). The team has been able to process and settle **over 100,000 blocks** through downstream services across different networks—a significant achievement.

**Cairo Native Performance:**

Detailed report coming, but preliminary data shows **2-3x gains over Cairo VM**. This is significant for anyone running traces.

**Madara Sync Progress:**

- **Sepolia**: Synced to tip
- **Integration network**: Syncing in progress (received feeder gateway access from Starkware)
- **Mainnet**: Some class hash mismatch on earlier blocks being investigated

**RPC 0.10.1 Progress:**

Working on RPC 0.10.1 spec. The major blocker was starknet-rust, so the team is now **contributing to starknet-rust** to ensure 0.10.1 compatibility. Initial PRs have been submitted. Madara with 0.10.1 support expected by end of week.

**Acknowledgment:**

Mohit thanked the Pathfinder team for providing a Sepolia RPC URL, enabling cross-node result verification.

**Upcoming Focus:**

Major priority is making **merkleization faster** for Madara. Data shows 60-80% of sequencing and full node syncing time is spent updating tries. Various approaches being explored.

### 4. Consensus Implementation Updates (Skipped)

Jasmina was not present—the third consecutive call without Malachite team attendance. Aayush noted he will make an announcement in the next call regarding this situation.

### 5. Any Other Business

**SNOS Changes for v0.14.2:**

Mohit asked Eitan about potential SNOS changes required for v0.14.2 due to client-side proving. Since Karnot maintains SNOS open source, they need to understand what changes will be needed.

Eitan acknowledged the question but did not have technical implications for SNOS on hand. He confirmed the Blockifier crate has adjustments for sequencers and full nodes to be compatible with these features. He committed to following up on any SNOS-specific requirements.

### 6. Closing Remarks

Aayush closed the call thanking everyone for joining and sharing updates.

**Reminders:**
- Next call: Thursday, February 12, 2026, 12:00 PM UTC
- Recording will be published on YouTube
- Meeting notes will be shared on GitHub

---

## Key Decisions Summary

1. **Network Confirmed Stable Post-Reorg**
   - No incidents since January 13, 2026
   - Postmortem published on starknet.io

2. **JSON RPC 0.10.1 Releasing Before v0.14.2**
   - Client releases will be compatible with future network version
   - Standard practice when nodes upgrade before network

3. **New Developer Crates Published for Testing**
   - Blockifier and Starknet API crates (dev versions)
   - Not for stable node releases

4. **Pathfinder Pre-Release with Client-Side Proofs Coming**
   - For testing only
   - Database migration required; no rollback without backup

5. **Juno Database Reduction Opt-In Release Imminent**
   - v0.15.18: Opt-in (~50% size reduction)
   - v0.16: Default migration (mid-February)

6. **Malachite Team Absent Third Consecutive Call**
   - Announcement planned for Call #45

---

## Action Items Tracker

| Owner | Action | Target Date/Call | Status |
|-------|--------|------------------|--------|
| Starkware | Continue staker engagement for decentralized validation requirements | Ongoing | In Progress |
| Starkware | Publish stable RPC crates for production use | Before next network release | Pending |
| [Eitan Moed](https://github.com/eitanm-starkware) | Follow up on SNOS changes required for v0.14.2 | Soon | Open |
| [Andrew](https://github.com/andrew-starkware) | Return for Propeller implementation discussion | Call #45 (if demand) | Pending |
| [Krisztian Kovacs](https://github.com/kkovaacs) | Release Pathfinder pre-release with client-side proofs | Soon | In Progress |
| Pathfinder team | Continue Propeller review and comparison with Turbine | Ongoing | In Progress |
| [Rodrigo Pino](https://github.com/rodiazet) | Release Juno v0.15.18 with database reduction (opt-in) | Today/Tomorrow | In Progress |
| [Rodrigo Pino](https://github.com/rodiazet) | Publish documentation for new database parameters | With release | In Progress |
| [Rodrigo Pino](https://github.com/rodiazet) | Release Juno v0.16 with default migration | Mid-February 2026 | Planned |
| Juno team | Begin Propeller and consensus implementation focus | February 2026 | Planned |
| Karnot team | Get SNOS v0.14.1 merged (pending Dojo review) | TBD | Blocked |
| Karnot team | Resolve mainnet class hash mismatch | This week | In Progress |
| Karnot team | Complete Madara RPC 0.10.1 support | End of week | In Progress |
| Karnot team | Publish Cairo Native performance report | TBD | Planned |
| Karnot team | Optimize merkleization performance | Coming weeks | In Progress |
| [Aayush Giri](https://github.com/Giri-Aayush) | Make announcement regarding Malachite team attendance | Call #45 | Planned |

---

## Attendees

- **Moderator:** Aayush Giri | Nethermind
- Chris | Equilibrium
- Eitan Moed | Starkware
- Krisztian Kovacs | Equilibrium
- Maksym Malicki
- Mohit | Karnot
- Rodrigo Pino | Nethermind
- Vaclav | Equilibrium

---

## Glossary

- **Reorg (Reorganization)**: Block rollback due to consensus issues; January 13 incident affected 507 blocks
- **Postmortem**: Incident analysis report; published on starknet.io
- **Observability**: Tools and systems for monitoring node performance and health
- **Key Management**: Solutions for securely managing validator signing keys
- **Sync Time**: Duration for nodes to synchronize with the network (initial or upgrade)
- **Apollo Packaging**: Bundling validator node components for community operators
- **Blockifier Crate**: Rust library for transaction execution; dev version published
- **Starknet API Crate**: Rust library for Starknet API; dev version published
- **JSON RPC 0.10.1**: Upcoming RPC specification version
- **v0.14.2**: Upcoming Starknet version with client-side proving support
- **Client-Side Proofs**: Feature allowing clients to generate transaction proofs
- **Concurrent Execution**: Parallel processing of transaction batches for improved performance
- **Turbine**: Solana's block propagation protocol; being compared to Propeller
- **Database Migration**: Process of converting database format; required for Pathfinder pre-release and Juno update
- **Opt-In Migration**: Optional database upgrade; user chooses when to apply
- **starknet-rust**: Rust SDK for Starknet; Karnot contributing for 0.10.1 compatibility
- **Merkleization**: Process of building Merkle trees for state; major performance bottleneck (60-80% of time)
- **Trie**: Data structure for efficient state storage; Juno working on improved version
- **SNOS (Starknet OS)**: Starknet operating system; maintained by Karnot
- **Encrypted DA**: Encrypted data availability solution
- **L3**: Layer 3 networks built on Starknet; incompatible with v0.14.1 two-proof system
- **Malachite**: BFT consensus engine by Informal Systems; team absent third consecutive call

---

**Next Meeting:** Thursday, February 12, 2026, 12:00 PM UTC

**Note:** These Starknet All Core Devs Calls occur bi-weekly at the same time. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.

**⏰ TIME REMINDER: Calls are at 12:00 PM UTC through the end of March.**
