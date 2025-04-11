# Starknet All Core Devs Meeting #24
## Meeting Details

- **Date & Time:** Thursday, April 10, 2025, 11:00-11:30 AM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/live/FajAn70HHRA
- **Agenda:** https://github.com/starknet-io/pm/issues/14
- **Moderator:** Aayush Giri

## Meeting Notes

The meeting began with Aayush Giri welcoming everyone and noting the permanent time change to 11:00 AM UTC. He outlined the packed agenda focusing on v0.13.5 deployment stability, Cairo Native progress, and integration work across the ecosystem.

### 1. Mainnet v0.13.5 Operation Status

Leonardo Lerer was initially unavailable to present, and this agenda item was deferred.

### 2. Cairo Native Implementation Progress

Ariel Elperin reported that replay testing for Cairo Native is progressing slower than expected, but the results remain promising with no differences identified with v0.3.4 so far. While he couldn't provide a firm date for testnet deployment, Ariel noted that once replay testing completes, enabling Cairo Native on testnet will require only configuration changes on the sequencer side. Regarding performance, he referenced the initial metrics from Lambda class showing approximately 2-2.5x improvement for transfers and up to 10-12x for more complicated contracts, though no newer benchmarks were available since the last call.

### 3. Client Team Updates

Krisztian Kovacs shared that Pathfinder has resolved the `starknet_getEvents` issues, which turned out to be a bug in database migration steps doe during updates. The team has confirmed that published database snapshots are correct for users upgrading to Pathfinder v0.16.3, and timeout issues have been fixed in this release as well. Work has begun on Malachite integration, including implementing the necessary context and adding it to the build system. Pathfinder's P2P code is progressing to enable participation in the consensus network and subscribe to relevant topics. Krisztian also highlighted their new tool for reconstructing L2 state diffs from L1 state, available in the [starknet-scrape](https://github.com/eqlabs/starknet-scrape/) repository under the EQLABS organization. They've also made progress on Cairo Native integration in Pathfinder, which will be offered as an opt-in feature when it becomes available.

Rodrigo Pino explained that Juno has implemented dynamic timeout handling for large blocks, which increases timeout duration based on retry failures. This feature is configurable through flags and fully documented. They've also added support for runtime timeout and logging changes via HTTP requests without requiring node restarts. The team is investigating one reported bug with `estimate_fee` under specific request conditions. Additionally, they're working on staking v2 support with improved signing and UX improvements. Rodrigo mentioned they've nearly completed work on adding block building capability to Juno, allowing it to function as a sequencer, which will be included as an experimental feature in the next release.

Nathan from Kasar Labs explained that while they aren't currently supporting v0.13.5, they are establishing a pipeline to recover to the latest Blockifier version, including upgrading SNOS to process the latest version blocks. The team's focus remains on stabilizing Madara for the Starknet stack, with P2P, consensus, and native implementation tracks temporarily paused. Significant progress has been made on these tracks, documented in PRs #463 for P2P, #487 for native, and #475 for Malachite. Nathan indicated plans to resume work on these tracks by the end of the month to prepare for upcoming versions.

### 4. P2P Protocol Chain ID Implementation

This agenda item was skipped as Shahak Shama was not available.

### 5. Malachite Consensus Integration

Adi Seredinschi reported significant progress on Malachite consensus integration. They've enhanced their interoperability testing setup with Papyrus to allow more dynamic network configurations, including the ability to define precise latencies ahead of experiment start time. The team has observed slower consensus finalization in heterogeneous networks with mixed sequencer implementations, which requires further investigation. Two(ADRs) have been completed and merged: ADR-003 on value propagation mechanisms, which focuses on block propagation approaches with Starknet using the streaming approach, and ADR-004 on the coroutine-based process for driving low-level APIs in Malachite.

Adi also mentioned they investigated the "timeout commit" feature and determined it likely won't be needed by implementation teams. The team has released Malachite v0.1.0, moving from patch releases to minor releases as APIs stabilize. They're planning a dedicated DevNet for long-running Malachite testing using a dummy sequencer built with Malachite, focusing specifically on stability and edge case coverage, separate from interoperability testing. The moderator's connection appeared to freeze toward the end of this section.

### 6. L1 Data Extraction Tool Progress

Vaclav announced the release of the L1 data extraction tool, which has been published and is available on GitHub. The tool supports all blob-based formats, including those with stateless and stateful compression, and works with all Starknet versions from v0.13.1 onwards. Currently at version 0.1.0, Vaclav noted there's potential for further optimization and extension. He encouraged users to try the tool and report any issues on GitHub.

### 7. AOB (Any Other Business)

No additional topics were raised. The meeting concluded with Aayush thanking participants and confirming the next call in two weeks.

-----
### Attendees

- Aayush Giri (Moderator)
- Adi Seredinschi @Informal
- Nathan | Kasar
- Krisztian | Equilibrium
- Vaclav | EQ
- Rodrigo Pino
- Chris | Eq
- Ariel Elperin
- Leonardo Lerer (L icon)
- Fireflies.ai Notetaker JB
- Trantorian (Kasarlabs)
- rian
- Eitan Moed
- Dat Duong
- Abel | EQ

------------
Note: These Starknet All Core Devs Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **April 24, 2025**, at **11:00 AM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.