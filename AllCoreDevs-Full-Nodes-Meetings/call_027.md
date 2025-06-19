# Starknet All Core Devs Meeting #27
## Meeting Details

- **Date & Time:** Thursday, May 22, 2025, 11:00-11:30 AM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/live/RfY-W62jQxw
- **Agenda:** https://github.com/starknet-io/pm/issues/17
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Notes

The meeting began with Aayush Giri welcoming everyone to the 27th Starknet All Core Devs call, outlining the focused agenda covering v0.14.0 integration progress, Cairo Native implementation, client team updates, and Malachite consensus implementation.

### 1. Starknet v0.14.0 Integration Progress

Leonardo Lerer provided a comprehensive update on the v0.14.0 integration progress. He confirmed that the stabilization period lasted only a couple of days, and by the following week after May 6th, client teams were already able to begin their integration work. Pathfinder had already released a pre-release version that week which syncs correctly on v0.14.0, and other teams have started working on integration as well.

Internally, Leonardo reported that the chain is now pretty stable with ongoing bug fixes here and there. There is continued development of new features, especially regarding transactional status functionality. He explained that they want to support statuses with more granularity between "received" and "accepted on L2," which correspond to the various phases that the block builder passes through, and they plan to support these at the feeder level. The decision about this enhanced status tracking was very recent, and Leonardo planned to update the client teams immediately and prepare a version two of the release notes that describes the changes. He emphasized that he will discuss all changes with each team individually.

Leonardo reminded everyone that they are deprecating all transaction versions in the upcoming v0.14.0 release. In particular, only transaction V3 sent with RPC 0.8 will be accepted, and all other transaction versions will be rejected at the gateway. He noted that they still have external partners upgrading their backend flows to send those transactions, with some having already finished and the vast majority ongoing and planned to finish soon, putting them in a good position overall.

Regarding the timeline for testnet deployment, Leonardo indicated that the timeline is around mid-June or slightly after, which roughly corresponds to the testnet deployment schedule.

### 2. Cairo Native Implementation Update

Leonardo provided updates on Cairo Native implementation since Ariel was not present. He explained that there isn't much to update in terms of steady state for Cairo Native, as they are currently letting it run and monitoring for any discrepancies between VM runs and Cairo Native runs. The purpose is to gather confidence, and once they have sufficient confidence, they plan to enable Native on mainnet in some way in the upcoming couple of months. The exact method for enabling it on mainnet is still to be determined.

### 3. Staking v2 Implementation Status

Aayush noted that Nathan could not join the call today, so the Staking v2 updates were skipped for this meeting.

### 4. Client Team Integration Updates

**Pathfinder** (Krisztian Kovacs):
- Already released a pre-release version that can successfully sync with the integration network, as mentioned by Leonardo
- Support for v0.14.0 is incomplete, lacking the compiler upgrade and blockifier upgrade required for deploying new classes and executing new code
- Effectively waiting on Starkware for the new release and will bump their blockifier version once ready
- Working on other features that prevent a non-pre-release yet, but this is acceptable for integration work with the upcoming Starknet version
- Plan to wait for blockifier upgrades and do the compiler upgrade once released
- Still missing the additional transaction status changes mentioned by Leonardo
- For Staking v2: Released new version of attestation tool yesterday, waiting on commitment of attestation contract address on mainnet, will likely do follow-up minor release so users don't have to manually configure contract address before mainnet upgrade
- Malachite integration: In progress, reached point where underlying P2P aspects are implemented and can start wiring up Malachite and consensus state machine with their P2P code

**Juno** (Rodrigo Pino):
- Just made small release of Juno v0.14.4 with couple of small bug fixes
- Regarding consensus: Aiming for mid-June to end of June to have implementation ready
- Goal is to set up small test chain of at least four Junos and test during July
- Had communications with Informal Systems who have done tests in their way, potentially planning shared testnet when ready along with other client teams

**Kasar Labs** (Trantorian):
- Currently support v0.13.5 block execution, added to development branch and will be merged into main in coming days
- Testing framework remains in same state as before - changes merged into main, Madara now supports running end-to-end Starknet Hive RPC tests
- Still need to fix few methods that don't pass these tests, but they have been integrated into Madara
- Currently starting work on v0.14.0 method support on RPC side
- Significant contribution by Kakarot team regarding L3 support, bootstrap, and orchestrator developments
- Current status: Testing framework integrated, v0.13.5 supported, v0.14.0 in works

### 5. Malachite Consensus Implementation

Adi Seredinschi provided detailed updates on recent releases and network testing progress. The recent releases included several important fixes, with the most significant being fixes for problems in the liveness ensuring protocols - protocols that guarantee when a node does not make progress, that node can catch up with other nodes. He clarified this is not sync (which typically involves catching up across multiple heights that were already decided) but catch-up within a round or within a height, specifically liveness ensuring protocols.

All details have been shared with Matan and his team, and on Malachite's side, they finished implementing all changes and retracted the old protocol that existed. They are waiting on the Starkware team to adapt the protos because it will require some changes in the struct proto files. Several other changes were made, some not yet released but some described have been released in v0.2.0. Other small changes were related to test code, including fixes for a couple of small bugs related to the P2P layer, quality of life improvements, small bugs in storage code, and evidence handling efficiency fixes. The last two have been released in v0.2.0, which was released approximately two weeks ago.

Regarding testing, Adi explained they are conducting two batteries of tests. The first is interoperability tests that have been ongoing for about two months. They put these on pause briefly but restarted last week. The main focus remains an issue they still haven't fixed: unexplained slowness when both the Papyrus and Malachite-based sequencer are acting as proposers (not simultaneously, but switching from one to the other in round-robin fashion). Block production becomes significantly slower, and it doesn't relate to configuration issues, which they've ruled out along with several other potential causes, but they don't know exactly what causes it yet.

For interoperability tests, they have other items to investigate but are blocked on supporting specific changes from the Papyrus framework used to generate blocks. They need to be able to generate non-empty blocks, which hasn't been investigated yet, and need to decide with Matan's team whether it's worthwhile to test rotating validator sets. Currently, the validator set is fixed and not very large, but when validators change, the network should continue functioning, and this rotating validator set functionality hasn't been tested.

The second battery of tests will be helpful when they have a testnet running. This involves writing scaffolding to manage the testnet, generate workload, observe metrics, and introduce network perturbations such as latencies, crashed nodes, or Byzantine nodes. This work is progressing well, and they actually found a bug in an implementation not used for Starknet but used by another user - the bug relates to block sync. The process of writing this testnet framework is being actively used to track down other issues and has proven useful.

### 6. Additional P2P Network Update

Since Shahak Shama was present, Aayush asked for updates on P2P network status, particularly regarding support for multiple sequencer architecture, mempool distribution implementation, and chain ID implementation status.

Shahak explained that for multiple sequencer architecture, they've published the Starkware sequencer node named Apollo and the protobufs they're planning to use in consensus, available in the specs. He noted that Rian asked some questions about it, and that represents the plan for how multiple sequencers will communicate.

Regarding chain ID in discovery, Shahak expressed indifference about whether it's needed or not, stating that if teams think they need it, he'll approve a pull request to add it to the specs.

For mempool distribution, Shahak clarified that the plan for v1.0 (the version after v0.14.0) is that Starkware's node will be the only proposer, with other nodes serving as validators that vote on blocks the proposer produces. They don't plan to have mempool distribution in v1.0 because mempool is only needed for proposing transactions, not for validating them.

### 7. AOB (Any Other Business)

No additional topics were raised by participants.

-----
### Attendees

- Aayush Giri (Moderator)
- Rodrigo | Nethermind
- Vaclav | EQ
- Krisztian | Equilibrium
- Shahak Shama & Eitan Moed
- Adi Seredinschi @Informal
- Leonardo Lerer | Starkware
- Chris | Eq
- JB | Kasar
- Trantorian (Kasarlabs)
- cchudant
- Nikša (EQ)

------------
Note: These Starknet All Core Devs Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **June 5, 2025**, at **11:00 AM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.
