# Starknet All Core Devs Meeting 
## Meeting Details

- **Date & Time:** Thursday, January 16, 2025, 12:00-12:30 PM UTC
- **Duration:** 30 minutes
- **YouTube:** https://youtube.com/live/93t37QP8DME
- **Agenda:** https://github.com/starknet-io/pm/issues/8
- **Moderator:** Aayush Giri

## Meeting Notes

The meeting began with Aayush Giri welcoming everyone to the first core dev call of 2025 after the holiday break, setting the stage for several important updates, particularly around version 0.13.4.

### 1. v0.13.4 Implementation Overview

[Ariel Elperin](https://github.com/ArielElperin) provided a detailed update on version 0.13.4 implementation, focusing on the Transaction Executor API. While initially expressing some previous pessimism, Ariel clarified that the blockifier has become more dominant in the new version. The Transaction Executor API, situated in [transaction_executor.rs](https://github.com/starkware-libs/sequencer/blob/3da1b6b925a1178f94d3f4a3404c3cf2d20c9e9a/crates/blockifier/src/blockifier/transaction_executor.rs#L25), provides the [execute_txs_sequentially](https://github.com/starkware-libs/sequencer/blob/3da1b6b925a1178f94d3f4a3404c3cf2d20c9e9a/crates/blockifier/src/blockifier/transaction_executor.rs#L231) function that handles all state setup processes. This represents a significant improvement over the current system where nodes must manually initialize state, block context, and handle pre-processing methods.

Ariel explained that the executor is designed to encapsulate all execution processes, aiming to align nodes on execution code using the blockifier through a clear, self-contained API. However, he noted current limitations regarding transaction-level state diffs, which the executor doesn't yet expose. Two potential solutions were presented: including these in a subsequent blockifier RC, or continuing with existing execution methods that don't rely on the executor.

Regarding system contracts, Ariel emphasized that the current implementation is robust since contracts cannot access contract address storage, eliminating potential discrepancies between nodes and sequencer execution. The RC0 release is expected imminently, with the main bottleneck being the simplification of the blockifier build process due to LLVM dependencies.

### 2. New Resource Model Updates

[Leonardo Lerer](https://github.com/leo-starkware) presented comprehensive changes to the resource model in the new starknet version. The key innovation is the introduction of L2 gas as a resource that encompasses all execution-related resources, including what was previously known as steps and builtins, along with blockchain resources such as call data and events. Under this new model, L1 gas is reserved exclusively for messages from L2 to L1 and for DA mode call data.

Leo detailed the new sierra version's gas metering mechanism, explaining how it differs from the VM implementation. While the VM used maximum resources based on stone proofs and layouts, the new Sierra gas metering functions as a counter, summing all steps consumed and action tariffs. This change results in more accurate pricing as it's calculated per trace cell rather than per layout.

A technical challenge was highlighted regarding fee estimation with the new gas metering system. Leo explained situations where the final gas counter might be smaller than the maximum during execution due to gas redeposits. This requires client teams to implement specific workarounds, similar to Ethereum's handling of gas redeposits. He directed teams to review Ariel's detailed post in the full node collab channel for implementation guidance.

### 3. Client Implementation Updates

[Krisztian](https://github.com/kkovaacs) reported on Pathfinder's progress, noting their active work on integrating the new blockifier version and implementing changes required for syncing with the integration network. The team has updated their public integration node with the latest snapshot version supporting integration syncing and initial execution capabilities for generating traces. Current challenges include implementing the newly discussed fee estimation approach and handling gas estimates for in-know calls in transaction traces. They are also working to incorporate recent JSON RPC 0.8 specification changes, including the removal of blockId from subscribe transaction status.

[Rodrigo](https://github.com/RodrigoPC) shared updates from the Juno team, indicating they are awaiting the official blockifier release before beginning integration work. He reported completion of all WebSocket implementation work, with only get storage proof pending final review. The team is currently investigating system contract-related issues in testnet, with more information to follow.

The Kasar Labs team, represented by [Antiyro](https://github.com/antiyro), announced several significant developments. They have successfully released the Starknet stack, enabling custom Starknet appchain deployment with complete functionality. Regarding core development, they are working on stabilizing Madara for the stack deployment and are missing only three subscription RPC methods before having full coverage. Cchudant provided additional detail on their P2P implementation progress, reporting successful node syncing capabilities and ongoing work toward transaction and block propagation. This progress positions them well for future Malachite integration, though they acknowledge the implementation is still in early stages.

### 4. Wallet and API Integration Updates

The discussion then turned to wallet and dapp API changes. A Starkware representative detailed the migration status, noting that major wallets including Braavos, Argent, OKX, and MetaMask have already implemented the new API. The update introduces a `WalletAccount` object that combines wallet object and provider functionality, abstracting API communication through a message protocol between wallet and dapp. This new architecture reduces version coupling of StarknetJS on both frontend and wallet sides, with wallets now handling fee calculations and transaction versioning independently. The transition has been live with broad adoption across the ecosystem.

### 5. Core Dev Summit Discussion

The meeting concluded with an announcement about the upcoming Core Dev Summit, scheduled for January 27-28, 2025, in Warsaw. A draft agenda is being prepared and will be shared with the group. The organizers emphasized the importance of proper preparation and agenda alignment to maximize the effectiveness of the in-person discussions.

### 6. Next Meeting Schedule

The next meeting is scheduled for January 30, 2025, at 12:00 PM UTC.

-----
### Attendees

- Aayush Giri
- Shahak Shama
- Leonardo Lerer
- Rodrigo Pino
- Denisa Diaconescu
- Krisztian 
- Llewellyn
- Trantorian
- Fireflies.ai
- Vaclav
- Eftan Moed
- Dan Brownstein
- Toni
- Daniil Arkushin
- amanusk
- JB
- Nathan
- cchudant
- Adi Seredinschi
- rian
- Carmen
- Ariel Elperin

------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for Wednesday, **January 30, 2025**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.