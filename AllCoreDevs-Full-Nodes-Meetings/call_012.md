# Starknet All Core Devs Meeting 
## Meeting Details

- **Date & Time:** [Thursday, October 10, 2024, 11:00-11:30 AM UTC](https://www.timeanddate.com/worldclock/converter.html?iso=20241010T110000&p1=1440&p2=37&p3=136&p4=237&p5=923&p6=204&p7=671&p8=16&p9=41&p10=107&p11=28&p12=438)
- **Duration:** 30 minutes
- **YouTube:** [https://www.youtube.com/watch?v=vvHvVMeUgRw](https://www.youtube.com/watch?v=vvHvVMeUgRw)
- **Agenda:** [https://github.com/starknet-io/pm/issues/1](https://github.com/starknet-io/pm/issues/1)
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)


## Meeting Notes

### 1. Starknet v0.13.3 readiness updates

- Juno (update by [Kirill Paltsev](https://github.com/kirugan)): They haven't started new block hash implementation but have almost finished all RPC endpoints, with three PRs in review stage. They've begun work on several WebSocket endpoints, not expecting completion before the next full node clients call but anticipating some merges before then.

- Kasar Labs (update by [Antiyro](https://github.com/antiyro)): Still building their sequencer, not focusing on 0.13.3. They've implemented a Feeder Gateway compatible with latest specs, able to sync with Juno and Pathfinder. They've started 0.8.0 integration and dropped comments on the getProof endpoint. WebSocket endpoint implementation is next. The Feeder Gateway is available as a Docker image, supporting sequencer and devnet modes with pre-funded accounts, and can synchronize as a full node for testing with Juno.

- Starkware (update by [Leonardo Lerer](https://github.com/leo-starkware)): No definitive timeline yet, but details will be communicated formally later. Tentative timeline may look like: integration starting early November, one month for remaining integrations, minimum one month on Sepolia testnet, mainnet upgrade targeted for early January, probably. Main focus for the upcoming version is Cairo native integration, which may not be fully complete at initial integration. They plan to provide something for client teams to work with until it's fully upgraded to the final stage, with potential upgrades during the integration timeline.

### 2. P2P integration progress

- Pathfinder (updated by [Leonardo Lerer](https://github.com/leo-starkware) on behalf): Implemented new block hash calculation algorithm for all blocks for P2P sync, so their endpoint hsould be ready for testing and sycn with old blocks as well, including historical ones. Feature believed to be merged into main branch, pending confirmation.

- Juno: 
(update by [Kirill Paltsev](https://github.com/kirugan)) Parallel implementation of new block hash calculation. Currently in pending Pull Request. Verified hash correctness for historical blocks, ensuring backwards compatibility.
(update by [Mikolaj Barwicki](https://github.com/stranger80)): Developing live compatibility dashboard for cross-client testing. Aims to provide real-time visibility of inter-client compatibility status.Its in very early stage but it has Potential for integration into Starkware repositories for wider access.

- Kasar Labs ([Antiyro](https://github.com/antiyro)): Prioritizing sequencer development. Production readiness targeted for current month, focusing on Pragma chain integration. P2P client development scheduled as subsequent phase.

### 3. Consensus implementation progress and design research discussions:
#### Research
[Denisa](https://github.com/denisadiaconescu) published a post on the Starknet Community forum on research involving SOS blocks, aimed at helping the protocol recover from unprovable blocks, which is crucial for protocol liveness. The research demonstrates, how SOS blocks can be used during consensus.
- Key points:
    - Assuming K proof chains in the protocol, the next K-1 blocks built on top of an SOS block can piggyback proofs already included in blocks between the unprovable block and the SOS block.
    - By manipulating ZK proofs, forks can be avoided in cases of unprovable blocks.
    - The chain can continue building blocks without deleting the affected part containing an unprovable block, until an SOS block is reached.
    - Applications only need to roll back the state to the point before the unprovable block.
    - This solution is considered the least invasive for dealing with unprovable blocks.

The team is conducting experiments on building ZK proofs needed for SOS blocks. The consensus part and protocol recovery seem straightforward. Importantly, L1 state updates are not affected. The diagram shows how the same mechanism for computing L1 state updates can be maintained by merging k-ZK proofs found in the last proven blocks from each proof chain.

#### Implementation
(update by [Aneeque Safdar](https://github.com/IronGauntlets)):
The team is following a similar approach to Informal Systems, starting with Tendermint implementation and then adding parts tailored for the Starknet protocol. Current progress:
Majority of the consensus algorithm has been implemented.
Focus is on adding unit tests, particularly for important state transitions.
Aim to wrap up this phase by next week.
Next step is to start using their sequencer to begin propagating blocks and achieving consensus.

Additional points (by [Denissa](https://github.com/denisadiaconescu)):
Planning meetings with Informal Systems to synchronize progress and address slashing conditions.
Starkware prefers minimal slashing conditions to incentivize stakers, but at least one slashing condition is necessary to maintain consensus.
This balance between economic incentives and consensus preservation needs further examination.

Implementation challenges (discussed with [Leonardo Lerer](https://github.com/leo-starkware)):
A key component of the recovery protocol involves running a Cairo execution and proving it in a ZK VM.
The team is experimenting with breaking this process into smaller chunks to overcome ZK VM limitations.
They've discovered a similar approach used by another team and are exploring that protocol and approach.
The team is more optimistic now than a few weeks ago about solving this challenge.
The main focus is identifying the minimal part of Cairo needed to build the trace and either run it in a ZK VM or produce a ZK proof for it.
This is currently the only missing ingredient in their implementation.

### 4. RPC standardization:
No specific issues or resolutions addressed in the meeting.
### 6. Client discrepancies:
No specific issues or resolutions addressed in the meeting.

### 7. Staking updates - Stage 2 preparations:
- SNF compliance testing team: Update unavailable due to absence of team representatives.
- [Mikolaj Barwicki](https://github.com/stranger80) and [Matteo Lisotto](https://github.com/Oghma) from the Nethermind team shared their ongoing internal discussions about improving the staking mechanism for Stage 2. They've been considering Nathan's ideas from the previous meeting and exploring ways to enhance the reliability of monitoring staker nodes that are expected to remain online. Their main concern was the lack of a liveness check in the current proposal, which led them to develop a new approach leveraging the P2P network.

Key points of their proposal:

- Utilize the P2P network for monitoring staker nodes
- Leverage the Peer ID (PID) from the libp2p protocol
- Combine staker address with PID in an on-chain contract
- Use heartbeat messages or other P2P traffic to verify node status
- This would complement Nathan's idea of periodic on-chain transactions

The team sees this as a promising way to verify staker node online status using both on-chain data and P2P network activity. They plan to refine these ideas internally before proposing them for public discussion, aiming to gather feedback and potentially improve the overall staking mechanism for Stage 2.

### 8. AOB (Any Other Business):
No additional topics or concerns raised by participants.

-----
### Attendees
	
- Kirill Paltsev
- Leonardo Lerer
- Wojciech Zieba
- Robert Kodra
- Mikolaj Barwicki
- Llewellyn Morkel
- Denisa Diaconescu
- Matteo Lisotto
- Mario Apra
- Ariel Elperin
- Pawel Nowosielski
- Eitan Moed
- Charpa
- Antiyro
- Stefan Ipach
- Aneeque Safdar
------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **October 24, 2024**, at **11:00 AM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.

