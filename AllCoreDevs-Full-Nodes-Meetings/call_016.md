# Starknet All Core Devs Meeting 
## Meeting Details

- **Date & Time:** [Thursday, December 5, 2024, 12:00-12:30 PM UTC](https://www.timeanddate.com/worldclock/converter.html?iso=20241205T120000&p1=1440&p2=37&p3=136&p4=237&p5=923&p6=204&p7=671&p8=16&p9=41&p10=107&p11=28&p12=438)
- **Duration:** 30 minutes
- **YouTube:** [https://www.youtube.com/watch?v=-HWTvHMBQIQ&list=PLMXIoXErTTYW7_3FjybBzJXhfZwvSchPa&index=2&t=1298s](https://www.youtube.com/watch?v=-HWTvHMBQIQ&list=PLMXIoXErTTYW7_3FjybBzJXhfZwvSchPa&index=2&t=1298s)
- **Agenda:** [#6](https://github.com/starknet-io/pm/issues/6)
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Notes

### 1. Starknet v0.13.3 & v0.13.4 readiness updates:

Starkware (update by [Leonardo Lerer](https://github.com/leo-starkware)) reported that the v0.13.3 mainnet upgrade was completed successfully with no issues. Regarding v0.13.4, development continues with Cairo native integration confirmed for inclusion unless unexpected issues arise. The timeline remains unchanged with integration targeted for end of December, followed by one month for testing and integration, then one month on testnet, with expected mainnet deployment by end of February. RPC 0.8 availability is targeted for end of December.

In response to questions about Cairo native from [Antiyro](https://github.com/antiyro), Leonardo clarified that the implementation includes both the Lambda class compiler crate and blockifier integration. The blockifier includes struct for choosing between native or CASM compiled artifacts, with implementation teams needing to handle compiled contract caching/persistence. While code exists in the blockifier for native execution, its stability status is still being assessed, though the compiler crate is reported as relatively stable.

### 2. P2P Protocol Evolution:

[Shahak Shama](https://github.com/ShahakShama) presented updates on protocol naming convention, proposing a format of [network_spec]/[chain-id]/[sub-network]/[kad].. following libp2p convention of putting network specification first. The team has added protobuf specs for mempool and consensus networks and is planning CI implementation for P2P specs to ensure ongoing compatibility. The discussion included clarification on the flexibility in ordering of chain ID versus sync/mempool identifiers.

### 3. Client implementation updates:

- Pathfinder (update by [Krisztian Kovacs](https://github.com/kkovaacs)) has released version `0.15` with preliminary support for JSON RPC `0.8`. The team is implementing based on the recently released candidate and has received limited feedback, primarily regarding transaction hash placement in [getBlockWithReceipts](https://www.quicknode.com/docs/starknet/starknet_getBlockWithReceipts) response.

- Juno (update by [Kirill Paltsev](https://github.com/kirugan)) reported that [getStorageProof](https://github.com/NethermindEth/juno/pull/2194) remains their final endpoint to implement. The team is exploring internal trie implementation optimizations before merging and continues work on WebSocket implementation through open PRs. They aim to improve stability before finalizing the implementation.

- Kasar Labs (update by [Trantorian](https://github.com/Trantorian1)) has nearly completed their RPC 0.8 implementation. The team has merged `getStorageProof` implementation, with `getMessageStatus` currently being merged. Only 2-3 WebSocket methods remain to be implemented, and they are on track for v0.13.4 readiness. They have begun exploring P2P implementation possibilities, though detailed updates were deferred to the P2P section.


### 4. Testing Infrastructure:

[Mikolaj Barwicki](https://github.com/stranger80) provided an update on the P2P testing infrastructure. The public P2P cross-compatibility dashboard is now operational, currently testing Juno and Pathfinder full history sepolia sync. Teams are actively working to resolve red status indicators. Development of an L1 smart contract for publishing boot nodes is underway for the proposed tesnet, with deployment expected before Christmas.

Regarding launch criteria, Mikolaj proposed validating Juno and Pathfinder's ability to sync full Sepolia history through multiple successful tests. The proposal suggests requiring green status on like example 5-10 consecutive full Sepolia syncs before considering the implementation ready for public launch. A formal written proposal detailing these criteria will be shared with the community.

[Shahak Shama](https://github.com/ShahakShama) shared important context about the motivation behind creating the P2P testnet. The team is working on converting papyrus into a sequencer node (which may be renamed) as part of plans to transform main starknet's architecture. The new design will involve four sequencer nodes communicating in a decentralized manner to create new blocks. While the ultimate goal is complete decentralization of Starknet, an important intermediate milestone is eliminating dependency on the Feeder Gateway by enabling nodes to receive block data through peer-to-peer networking. This change will not only advance decentralization efforts but also reduce maintenance overhead by eventually allowing the team to deprecate the Feeder Gateway code. If the P2P testnet is not ready in time, the team will continue maintaining the Feeder Gateway alongside the sequencer nodes.

### 5. Sequencer Development:

[Shahak Shama](https://github.com/ShahakShama) outlined plans for v0.14.0, which will implement sequencer nodes with internal consensus, targeted for end of Q1 2025. The initial implementation will be controlled internally for security validation, with future plans for full decentralization allowing open sequencer participation. Key goals include decentralizing Starknet block production, eliminating feeder gateway dependency, and transitioning to P2P block data distribution.

### 5. Malachite/Consensus Implementation:

[Adi Seredinschi](https://github.com/adizere) from Informal Systems  provided an extensive update on the protocol design for decentralized sequencer and Malachite implementation (Tendermint in Rust). The team has made significant progress over the last month on several key components. They've completed the implementation of node discovery based on Kademlia and successfully tested it. A crucial addition is the Write-Ahead Log (WAL) with crash recovery support, allowing the network to recover even if more than two-thirds of nodes crash simultaneously.

The team has implemented two distinct synchronization mechanisms: block sync and vote sync. The latter was added after discovering its necessity for handling degraded network conditions, particularly when peers are stuck in different rounds during block creation. This addresses scenarios where peers might be stuck with a hidden lock in initial rounds while others attempt to progress to subsequent rounds. Rather than relying on inefficient large-buffer gossip networks, they've implemented a targeted vote synchronization mechanism.

Regarding accountable consensus, there's an [ongoing discussion](https://community.starknet.io/t/accountability-and-incentives-for-consensus/115031) on the Starknet Community forum focusing on whether to support protection against Amnesia attacks (a variant of long-range attacks). The team is seeking input from various stakeholders including full node teams, operators, core teams, indexers, wallets, and explorers to determine the best approach for integration.

The Malachite codebase is now ready for public release from an engineering perspective. The team is currently focusing on documentation, examples, and final refactoring to ensure the codebase is prepared for potential abuse scenarios, drawing from experiences with BFT. The public release is scheduled for mid-January 2024.

Next steps for the team include:
- Syncing with Shahak regarding node discovery implementation
- Collaborating with full node teams, particularly Kasar and Equilibrium, for Malachite integration
- Continuing the forum discussion on Amnesia attacks and gathering stakeholder input
- Preparing for the mid-January public release and announcement

The team emphasized that access to the codebase is already available for interested implementation teams, even before the public release.
-----
### Attendees

- Aayush Giri (Moderator)
- Leonardo Lerer
- Shahak Shama
- Jorik
- Krisztian Kovacs
- Kirill Paltsev
- Trantorian
- Mikolaj Barwicki
- Adi Seredinschi
- Wojciech Zieba
- Toni Tabak
- Matteo Lisotto
- Vaclav
- Chris
- Abel E
- penovicp
- Llewellyn Morkel
- Denisa Diaconescu
- cchudant 
- Aneeque Safdar
- Mohit
- amanusk
- rian
- Jean-Baptiste Caron
- Pawel Nowosielski
- Ariel Elperin
- Daniil Ankushin
- Tomasz Stanczak

------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **December 19, 2024**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.