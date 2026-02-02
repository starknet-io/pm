# Starknet All Core Devs Meeting #43
## Meeting Details

- **Date & Time:** Thursday, January 15, 2026, 12:00-12:45 PM UTC
- **Duration:** 45 minutes (extended for Propeller presentation)
- **YouTube:** https://www.youtube.com/watch?v=1BKf3WRh1i8
- **Agenda:** https://github.com/starknet-io/pm/issues/33
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Screenshot

![Call #43 Attendees](images/call_043_attendees.png)

## Executive Summary

This was the **first Starknet All Core Devs Call of 2026**, featuring an extended session for a special presentation on Propeller, a new P2P messaging protocol. Key outcomes included:

- **2026 Roadmap announced: v0.15 decentralized validation by end of Q3 2026** - Apollo setup with community validators voting on blocks; step-by-step progression through 0.14.x versions
- **Mainnet reorg incident disclosed** - Occurred January 13, 2026; caused by batching/proving service disagreement due to incomplete revert handling; 507 blocks affected; quickly patched
- **Propeller P2P protocol presented** - New high-throughput block propagation protocol using Reed-Solomon error correction; achieves 10 MB/s with 50 nodes; designed for Tendermint consensus
- **Pathfinder v0.21.5 released** - Fixed blockifier dependency for reorg and HTTP/2 syncing bug; Q1 focus on consensus implementation
- **Juno targeting 40% database size reduction** - Migration release expected end of January; improved trie coming Feb/March
- **Madara/Orchestrator v0.14.1 fully released** - Cairo Native showing strong performance; focus on getting full node to mainnet tip
- **Jasmina absent** - Consensus updates deferred again

## Meeting Notes

The meeting opened with [Aayush Giri](https://github.com/Giri-Aayush) welcoming everyone back from the holidays to the first Starknet All Core Devs Call of 2026. He covered housekeeping items including the recording, muting protocol, and reminded everyone that the 12:00 PM UTC time slot remains effective through end of March. He noted this was a special extended session running approximately 45 minutes to accommodate the Propeller presentation.

### 1. 2026 Roadmap and Decentralized Validation Plans

Aayush began by recalling that in the last call (December 18, 2025), Eitan mentioned they were evaluating 2026 plans and would have a published roadmap ready by this call.

[Eitan Moed](https://github.com/eitanm-starkware) confirmed they have a rough outline. While he did not have a formal presentation, he shared the key milestones they are working towards.

**v0.15: Decentralized Validation Target**

The goal is to have decentralized validation by **end of Q3 2026**. This means all blocks introduced to the network will be voted on by a decentralized set of validators. This flagship version will be called **v0.15**.

**Apollo Setup Architecture**

Decentralized validation will be introduced in an "Apollo" setup:
- **Apollo proposers** run by Starkware proposing blocks
- **Apollo validators** run by the community validating these blocks

**Incremental 0.14.x Versions**

Multiple 0.14.x versions will be released leading up to v0.15, each taking small steps toward the decentralization goal. The team is still shaping the exact technicalities for these intermediate versions.

**Key Milestones Being Worked On:**
- Setting up staking rewards based on participation in consensus (attestations)
- Packaging Apollo so validators can run standalone nodes without needing a gateway or mempool
- Validators will read transactions from the consensus P2P network and validate them

**Decentralized Proposing**

Later in 2026, toward the end of the year, work will begin on the beginnings of decentralized proposing. Technical details are not yet finalized as they are still discussing the step-by-step approach.

**Lessons from v0.14.0**

Eitan emphasized they are learning from v0.14.0, which was a very ambitious upgrade. The key lesson is to spread goals into smaller "baby steps" to incrementally and surely reach each milestone.

### 2. Post-Deployment Network Status and Reorg Incident

Aayush asked for an update on the 502 errors from the December deployment.

**DNS Caching Resolution**

Eitan explained the DNS caching issue: Starkware recommends a certain cache duration, but servers cannot force clients to adhere to this recommendation. Users who cached DNS longer than recommended experienced extended access issues. For future upgrades, they are considering backend workarounds to help users who may not have followed the recommendations.

**Mainnet Reorg Incident (January 13, 2026)**

Eitan disclosed a significant incident that occurred the previous Monday (January 13, 2026).

**What Happened:**
- A disagreement was detected between the batching service and the proving service
- A transaction was executed but could not be proven
- This is the validity proof system working as intended—nothing illegitimate gets settled to L1

**Response:**
- The network was taken down as quickly as possible to investigate
- The reorg occurred after **507 blocks** (each block created within ~2 seconds)

**Root Cause:**
- In execution, when a contract performs a specific flow of reverting—going into functions, changing values, reverting out, and going back in—there was a very specific case where the revert on one call did not cover all parameters
- A specific field of a specific function was not reverted, causing the mismatch between execution and proving

**Resolution:**
- The bug was quickly patched and deployed
- The network was restored
- A postmortem will be shared with the community if not already published

### 3. Propeller: New P2P Messaging Protocol Presentation

Aayush invited [Andrew](https://github.com/andrew-starkware) from Starkware to present Propeller, a new P2P messaging protocol.

**The Problem: Block Propagation Limits TPS**

Andrew explained that block propagation—sending a proposed block to everyone in the network—dictates the theoretical maximum transactions per second for any decentralized blockchain. If blocks cannot be sent fast enough, TPS will be capped.

Benchmarks showed that gossip-sub (used by Ethereum and currently by Starknet) is the most common protocol but is not designed for high throughput by one broadcaster. It will not scale to Starknet's TPS targets.

**Propeller Overview**

Propeller is a new protocol developed by Starkware, heavily inspired by Solana but adapted for Starknet's specific needs:
- Uses **Reed-Solomon error correction codes** for data redundancy
- Distributes network bandwidth among all validators
- Designed specifically for **Tendermint consensus**
- Achieves the **gossip property** required by Tendermint

**How Propeller Works**

1. **Sharding**: The proposer divides the message into F equal "data shards" (where F = floor((N-1)/3), the maximum faulty nodes)
2. **Error Correction**: Additional ~2F shards are added using Reed-Solomon coding, so any F shards can reconstruct the full message
3. **Distribution**: Each validator receives one shard and broadcasts it to all others
4. **Reconstruction**: Once a validator has F shards, they can build the message; once they have 2F shards, they can "receive" it (pass to Tendermint)

**The Gossip Property**

The 2F threshold ensures the gossip property: if one good peer receives a message, all good peers will eventually receive it. This prevents locking attacks where validators lock on different blocks.

**Performance Results**
- 50 nodes, 10 MB/s throughput
- 180ms latency
- Extremely stable at these rates
- Two-hop message delivery (faster than gossip-sub)
- Each participant needs 3x the message size in bandwidth

**Votes Handling**

Votes (many-to-many, small messages) will use **flood-sub** (one-hop) rather than Propeller, as throughput efficiency matters less for small messages.

**External Review**

Propeller has been reviewed by **Informal Systems** from a theoretical perspective.

**Specification and Multi-Language Support**

Discussion ensued about cross-language support:

[Rodrigo Pino](https://github.com/rodiazet) raised concerns about Rust-centricity, noting Juno cannot implement in Rust. He requested:
- Published specifications
- Potentially a mono-repo similar to libp2p with specs that any language can implement

Andrew confirmed:
- Propeller is built on top of libp2p, making Go implementation easier
- Specifications will be published once implementation details are finalized
- The code is currently in the sequencer repo in its own crate
- They are considering publishing to libp2p through proper channels

Both teams agreed to communicate closely on updates to ensure multi-language implementations stay synchronized.

**Technical Q&A**

A question was raised about how validators know which subset of F shards to use for reconstruction. Andrew and [Shahak Shama](https://github.com/ShahakShama) explained:
- A Merkle root of all shards is signed by the proposer
- Each shard includes a proof of inclusion in the Merkle tree
- Validators verify no foul play occurred when building messages
- Only the proposer can sign the root, preventing malicious shard injection

### 4. Client Team Updates

Aayush transitioned to client team updates, asking teams to keep updates to 3-4 minutes due to time constraints.

#### **Pathfinder** ([Krisztian Kovacs](https://github.com/kkovaacs)):

Krisztian reported two releases since the last call:

**Release 1 - Blockifier Fix:**
Related to the mainnet reorg incident, they upgraded the blockifier dependency to produce the same reverted transactions as the sequencer.

**Release 2 - HTTP/2 Bug Fix (v0.21.5):**
After Starkware enabled HTTP response compression on mainnet, Pathfinder nodes had syncing issues. The HTTP client stopped sending requests. The root cause was a bug in the underlying H2 crate, fixed in a late December release. Pathfinder v0.21.5 includes this fix.

**Q1 2026 Priorities:**
- Primary focus: **consensus implementation** for distributed validation
- Goal: provide a package for running validator nodes in the distributed consensus environment
- Secondary: keeping up with 0.14.x releases

#### **Juno** ([Rodrigo Pino](https://github.com/rodiazet)):

**January Focus - Database Size Reduction:**
- Targeting **~40% reduction** in database size
- Migration release expected by **end of January**
- Requires some implementation changes currently in progress

**February/March - Improved Trie:**
- Working on keeping state in a more efficient form
- Will enable future improvements like snap sync

**Other Q1 Work:**
- P2P focus after database work
- Propeller integration planning
- Decentralization pipeline work (execution optimization)

#### **Karnot/Madara** ([Heemank Verma](https://github.com/heemankv)):

**v0.14.1 Releases Complete:**
- Madara sequencer and full node v0.14.1 fully released
- Orchestrator v0.14.1 released and performing well after extensive battle testing
- Many orchestrator fixes implemented to increase confidence

**SNOS Status:**
- Pre-release for v0.14.1 available
- Waiting for Dojo team to review PRs before full release

**Cairo Native:**
- Extensive testing in December/January
- Performing very well on Madara with significant performance gains
- May publish a performance report (not confirmed)

**2026 Focus:**
- Getting Madara full node to the tip of Sepolia and mainnet
- Using snapsync feature shipped last year for faster sync
- Keeping up to date with all Starknet updates now that Madara is in sync

### 5. Consensus Updates (Skipped)

Jasmina was not present, so consensus updates were skipped. This marks the second consecutive call without consensus updates.

### 6. Any Other Business

Aayush opened the floor for any other topics. No additional items were raised.

### 7. Closing Remarks

Aayush closed the call thanking everyone for joining the first call of 2026. He offered special thanks to Andrew for the Propeller presentation, calling it exciting new infrastructure work for the network.

**Reminders:**
- Next call: Thursday, January 29, 2026, 12:00 PM UTC (back to regular 30-minute format)
- Recording will be published on YouTube
- Meeting notes will be shared on GitHub

---

## Key Decisions Summary

1. **v0.15 Decentralized Validation Target: End of Q3 2026**
   - Apollo setup: Starkware proposers + community validators
   - Validators will vote on all blocks
   - Step-by-step progression through 0.14.x versions

2. **Staking Rewards to be Based on Consensus Participation**
   - Attestations will determine rewards
   - Part of the 0.14.x milestone progression

3. **Standalone Validator Packaging Planned**
   - Validators will not need gateway or mempool
   - Will read from consensus P2P network

4. **Propeller Protocol to Replace Gossip-Sub for Block Propagation**
   - Higher throughput (10 MB/s with 50 nodes)
   - Two-hop delivery for lower latency
   - Specifications to be published

5. **Flood-Sub for Consensus Votes**
   - Small messages will use one-hop flood-sub
   - Propeller reserved for large block data

6. **Mainnet Reorg Incident Resolved**
   - Root cause: incomplete revert handling in specific execution flow
   - Patched and deployed; postmortem to be shared

---

## Action Items Tracker

| Owner | Action | Target Date/Call | Status |
|-------|--------|------------------|--------|
| Starkware | Publish 2026 roadmap details publicly | Q1 2026 | In Progress |
| Starkware | Share reorg incident postmortem with community | Soon | Pending |
| Starkware | Publish Propeller specification | Coming months | In Progress |
| [Andrew](https://github.com/andrew-starkware) | Coordinate with client teams on Propeller code access | Soon | Open |
| [Andrew](https://github.com/andrew-starkware) | Consider publishing Propeller to libp2p | TBD | Under Consideration |
| [Krisztian Kovacs](https://github.com/kkovaacs) | Continue consensus implementation for distributed validation | Q1 2026 | In Progress |
| [Rodrigo Pino](https://github.com/rodiazet) | Release Juno with ~40% database size reduction | End of January 2026 | In Progress |
| [Rodrigo Pino](https://github.com/rodiazet) | Implement improved trie for efficient state storage | Feb/March 2026 | Planned |
| Juno team | Plan Propeller integration | Q1 2026 | Planned |
| [Heemank Verma](https://github.com/heemankv) | Get SNOS v0.14.1 full release (pending Dojo review) | Soon | Pending |
| [Heemank Verma](https://github.com/heemankv) | Get Madara full node to tip of Sepolia/mainnet | Q1 2026 | In Progress |
| Karnot team | Consider publishing Cairo Native performance report | TBD | Under Consideration |
| [Jasmina Malicevic](https://github.com/jmalicevic) | Provide Malachite/consensus update | Call #44 | Pending |

---

## Attendees

- **Moderator:** Aayush Giri | Nethermind
- Andrew | Starkware
- Chris | Equilibrium
- egecaner
- Eitan Moed | Starkware
- Heemank Verma | Karnot
- Krisztian Kovacs | Equilibrium
- Maksym Malicki
- Nikša | Equilibrium
- Rodrigo Pino | Nethermind
- Shahak Shama | Starkware
- Vaclav | Equilibrium

---

## Glossary

- **v0.15**: Target Starknet version for decentralized validation; planned for end of Q3 2026
- **0.14.x versions**: Incremental releases leading to v0.15 with progressive decentralization features
- **Apollo Setup**: Initial decentralization architecture with Starkware proposers and community validators
- **Apollo Proposers**: Block proposers run by Starkware in the initial decentralized validation phase
- **Apollo Validators**: Community-run validators that vote on proposed blocks
- **Decentralized Validation**: Validators voting on blocks; target Q3 2026
- **Decentralized Proposing**: Community members proposing blocks; target late 2026
- **Propeller**: New P2P block propagation protocol developed by Starkware; uses error correction codes
- **Gossip-Sub**: Current P2P protocol used by Ethereum and Starknet; not designed for high throughput
- **Flood-Sub**: One-hop broadcast protocol; will be used for consensus votes
- **Reed-Solomon**: Error correction code used in Propeller; allows reconstruction from partial data
- **Gossip Property**: Network property ensuring if one good peer receives a message, all good peers eventually receive it
- **F (Faulty nodes)**: Maximum number of malicious nodes tolerated; calculated as floor((N-1)/3)
- **2F Threshold**: Number of shards needed to "receive" a message in Propeller; ensures gossip property
- **Merkle Root**: Cryptographic hash of all shards; signed by proposer to prevent manipulation
- **Tendermint**: BFT consensus protocol that Propeller is designed to support
- **Reorg (Reorganization)**: Rolling back blocks due to consensus disagreement; occurred January 13, 2026
- **Batching Service**: Starkware component that batches transactions into blocks
- **Proving Service**: Starkware component that generates validity proofs; detected the execution bug
- **Blockifier**: Transaction execution component; updated in Pathfinder for reorg compatibility
- **H2 Crate**: Rust HTTP/2 library; bug caused Pathfinder syncing issues
- **HTTP Response Compression**: Feature enabled by Starkware that exposed H2 bug
- **Pathfinder v0.21.5**: Latest Pathfinder release with HTTP/2 and blockifier fixes
- **Snapsync**: Madara feature for fast synchronization to chain tip
- **Improved Trie**: Juno optimization for efficient state storage; enables future snap sync

---

**Next Meeting:** Thursday, January 29, 2026, 12:00 PM UTC

**Note:** These Starknet All Core Devs Calls occur bi-weekly at the same time. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.

**⏰ TIME REMINDER: Calls are at 12:00 PM UTC through the end of March.**
