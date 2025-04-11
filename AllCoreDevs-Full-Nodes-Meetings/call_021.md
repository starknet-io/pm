# Starknet All Core Devs Meeting #21
## Meeting Details

- **Date & Time:** Thursday, February 27, 2025, 12:00-12:30 PM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/watch?v=x84vu_AJlkM
- **Agenda:** https://github.com/starknet-io/pm/issues/11
- **Moderator:** [Leonardo Lerer](https://github.com/leo-starkware) 

## Meeting Notes

The meeting began with Leonardo Lerer requesting participants to turn on video if possible, before moving directly into the agenda items focused on v0.13.4 testnet upgrade, Cairo Native updates, consensus implementation, and P2P progress.

### 1. Testnet v0.13.4 Deployment

[Leonardo Lerer](https://github.com/leo-starkware) reported that the testnet was successfully upgraded the day before, with only a few issues identified around simulation. The two specific issues mentioned were:

1. A division by zero error occurring with L2 gas calculations and Max L2 gas parameters when using Pathfinder for simulation. Leonardo noted he was unsure if this affected other client implementations.

2. A discrepancy between simulated gas costs and actual execution gas requirements. Specifically, a vanilla Starknet transaction `validate` function in simulation reportedly costs 25 gas units, but when submitted to the network with this limit, it errors with a message indicating 90 gas units are required. This issue is not related to L2 gas or fee transfers and affects old RPC versions and classes.

Leonardo emphasized they were investigating these issues and invited teams to share any additional findings. He noted that apart from these simulation concerns, the upgrade to the latest node versions had proceeded smoothly for RPC providers.

Regarding the mainnet upgrade timeline, Leonardo indicated they were targeting mid to late March, though no definitive date had been finalized yet.

### 2. Cairo Native Transition

[Leonardo Lerer](https://github.com/leo-starkware) provided an update on Cairo Native progress, noting that Lambda class had released version 0.32, which significantly refactors array handling to eliminate array copying during write operations. This change makes operations involving arrays much more efficient.

While block replay testing with this version went well, the team encountered an issue with dictionary handling just before integration. The problem is similar to arrays, where dictionaries are copied when snapshots are taken. However, unlike arrays, there is no functional use case for dictionary snapshots in Cairo currently, as any dictionary operation results in a write. Since no read operations occur on dictionary snapshots, the team is implementing a more efficient approach that simply passes pointers without copying.

Leonardo explained they expect version 0.33 of Cairo Native to address this dictionary issue, hopefully representing the final optimization before enabling it in the sequencer. He confirmed that when they enable Native in the sequencer, they will release a new blockifier RC with the updated crate, though he noted this should be a transparent change for client teams since it's an internal implementation detail.

When [Krisztian Kovacs](https://github.com/kkovaacs) asked about the update process, Leonardo clarified they would release a new RC with accumulated changes, potentially earlier if the fee estimation issues proved to be blockifier-related.

### 3. L1 Data Extraction Tool

[Leonardo Lerer](https://github.com/leo-starkware) introduced a topic not directly on the agenda but one he wanted to revive: extracting data from L1. He explained that current code floating around can decode blobs from v0.3.1 (the first Starknet version using Ethereum blobs) or decode calldata from older blocks, but multiple format changes have occurred since:

- v0.3.3 added compression
- v0.13.4 added stateful compression (using indexes for keys and contract addresses)

Leonardo proposed developing capability for nodes to read these blobs and state diffs from L1 transactions. He suggested it could be a separate binary shipped alongside nodes, which would:
1. Take a transaction hash or Starknet block number
2. Find the relevant state update transaction on L1
3. Read all blobs sent in that transaction
4. Perform FFT decoding to extract the contents
5. Apply appropriate decompression
6. Output the state format

He noted existing Rust code can handle FFT for a single blob, but needs extension to support compression and multiple blob handling. Leonardo emphasized this would benefit explorers like Blob Scan, which currently displays Starknet state updates as raw blobs without decoding.

With no objections raised, Leonardo interpreted the silence as agreement that this was worthwhile and relatively straightforward to implement. He mentioned he would share a reference page in the chat containing format changes across Starknet versions, though it was missing details about v0.3.2's introduction of multiple blob support.

### 4. RPC Compatibility Challenges

[Gorin](https://github.com/gorinskiy) raised a question about the default RPC endpoint, noting they had encountered issues with users sending RPC v7 requests to endpoints that had been updated to v0.8, causing compatibility breaks. He asked whether it might be worth removing the default endpoint and requiring users to explicitly specify the RPC version.

[Leonardo Lerer](https://github.com/leo-starkware) responded that he preferred maintaining the current approach, as it forces people manually using SDKs to notice their code needs updating. He explained this was the purpose behind not including old transaction versions in the v0.8 write API, creating a clear signal for developers to either update their code or explicitly add version identifiers to their URLs. He expressed confidence that automated services and wallets had already moved away from default endpoints, as they would have encountered and reported issues earlier if not.

### 5. Client Implementation Updates

[Nathan/Antiyro](https://github.com/antiyro) provided an update on Kasar Labs' implementation work, noting they have two advanced branches:
- One for P2P integration (#463)
- One for consensus support (#477)

Both branches are queued into their "Quaza" exploration effort, with the consensus branch depending on P2P implementation. He reported good progress, with synchronization with Informal Systems and other teams for P2P implementation. Nathan mentioned that the Madara network can now produce and verify blocks with Malachite.

The team is prioritizing P2P implementation since the consensus branch requires it, but they hope to have something interesting and ready for testing by the following month. He confirmed they are successfully syncing from Pathfinder and Juno via P2P, and are building images to forward to the Nethermind testing suite. While they expect some challenges, Nathan expressed confidence they were nearly finished with the P2P implementation.

### 6. Custom Version Constants

[Leonardo Lerer](https://github.com/leo-starkware) raised a final point about custom version constants, referencing an idea he had mentioned in the chat previously. He expressed admiration for Madara's approach of allowing overrides for version constants of any Starknet version, and wondered if this capability should be standardized across implementations.

He suggested a simple approach where a path would be specified via config or CLI, with a standardized format matching Starknet version constants for a single version, leaving it to implementations to decide which versions to override.

[Nathan/Antiyro](https://github.com/antiyro) agreed this could become part of the Starknet specs, noting it would benefit all options if teams aligned on this approach.

Leonardo concluded that this feature represented a "lower hanging fruit" than the L1 data extraction work, suggesting it might be worth prioritizing.

The meeting concluded without additional topics, wrapping up earlier than scheduled.

-----
### Attendees

- Krisztian 
- Leonardo Lerer
- Ariel Elperin 
- Denisa Diaconescu
- Llewellyn
- Carmen
- Nikša (EQ)
- Nathan | Kasar
- Gorin 

------------
Note: These Starknet All Core Devs Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **March 13, 2025**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.