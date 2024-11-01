# Starknet All Core Devs Meeting 
## Meeting Details

- **Date & Time:** [Thursday, October 24, 2024, 11:00-11:30 AM UTC](https://www.timeanddate.com/worldclock/converter.html?iso=20241024T110000&p1=1440)
- **Duration:** 30 minutes
- **YouTube:** [https://www.youtube.com/watch?v=qhKjI9v4SjA](https://www.youtube.com/watch?v=qhKjI9v4SjA)
- **Agenda:** [https://github.com/starknet-io/pm/issues/2](https://github.com/starknet-io/pm/issues/2)
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Notes

### 1. Starknet v0.13.3 readiness updates:

- Starkware (update by [Leonardo Lerer](https://github.com/leo-starkware)): Due to public holiday in their region, the Starkware team was absent with only administrative presence. Comprehensive updates on Cairo native integration progress postponed to the next meeting.

- Kasar Labs (update by [Antiyro](https://github.com/antiyro)): Quasar Labs is awaiting a stable release of Cairo native before implementation. Their technical approach involves developing a hybrid implementation in Madara that will enable users to switch to Cairo native when required for urgent scenarios. This flexibility aims to maintain system stability while enabling progressive adoption.

- Nethermind (update by [Rian Hughes](https://github.com/rianhughes)): Team has completed implementation of training methods, currently in final review stages. Their technical focus has shifted to WebSocket implementation, with development actively underway to expand functionality.

### 2. P2P integration progress:

- Nethermind (update by [Wojciech Zieba](https://github.com/wojciechos)): Havet started woirking on the compatibility dashboard yet. Juno team has been working on end to end P2P tests, cross client test but they report challenges with cross-client synchronization, with current tests mostly failing when syncing from other clients. They're implementing a new testing approach utilizing the madara sequencer for tests because they have recently added a nice feature with Feeder Gateway feature, enabling creation of private custom networks for controlled testing environments without requiring full Sepolia synchronization. While Juno-to-Juno synchronization functions correctly, cross-client compatibility requires further development.

- Equilibrium Labs (update by [Krisztian Kovacs](https://github.com/kkovaacs)): Pathfinder team has implemented modifications to commitments and hash algorithm versions, which may impact cross-client compatibility. They're initiating a technical investigation into synchronization issues with Juno, with direct collaboration planned between teams to resolve compatibility challenges.

### 3. Consensus implementation progress and design research:

[Denisa](https://github.com/denisadiaconescu): The team's Tendermint implementation is nearing completion, with current focus on validating against Informal Systems test suite for consistency. A key technical challenge involves obtaining the necessary ZK proof for SOS blocks - specifically investigating whether running the entire Cairo execution in a ZKVM is required. The team is analyzing the minimal Cairo components needed to build the trace and either execute it in a ZKVM or generate a ZK proof. They will verify their technical approach with [Leonardo Lerer](https://github.com/leo-starkware), particularly around the identification of essential Cairo components for trace construction and proof generation.

Regarding slashing conditions, the team has scheduled a meeting with Informal Systems to leverage their experience from Comet BFT regarding node misbehavior patterns. The discussion will focus on determining the minimum necessary slashing conditions that maintain protocol health while optimizing staker incentives. Team sees this balance between economic incentives and consensus preservation as crucial for protocol stability.

### 4. Staking updates - Stage 2 preparations:

[Matteo Lisotto](https://github.com/Oghma): Current implementation encountered a security vulnerability where malicious stakers could potentially manipulate the reward mechanism to zero out rewards for all participants. The proposed solution involves leveraging the P2P network infrastructure, specifically utilizing peer IDs for on-chain node activity verification. Technical review pending from [Mikolaj Barwicki](https://github.com/stranger80) and Juno team for implementation feasibility. Development approach considers future protocol stages to optimize resource allocation. Revised proposal with security fixes to be shared via the full node client teams Slack channel.

### 5. P2P Spec Versioning:

Technical discussion centered around the `starknet_subscribeNewHeads` endpoint implementation. Current specification lacks clarity regarding pending block parameter handling and associated error codes. Pathfinder's implementation suggests pending blocks should be invalid for this request, but formal specification needs updating, with continued discussion moving to Slack due to spec authors absence from the meeting. Shahak was not present at the meeting to provide the planned preview of the proposal for maintaining dual P2P specs. The discussion has been postponed.

### 6. AOB:

Kasar Labs ([Antiyro](https://github.com/antiyro)): First appchain in production - private testnet from Pragma. The technical implementation enables users to deploy custom chains, full nodes, and devnet environments through Madara. The system maintains compatibility with latest Starknet specifications while actively implementing v0.13.3 and v0.8.0. Notable technical achievement includes successful integration of EVM full node deployment capabilities on Starknet using Madara and Kakarot, with team providing implementation support.

-----
### Attendees

- Aayush Giri (Moderator)
- Leonardo Lerer
- Kristzian Kovacs
- Tomasz K. Stanczak
- Wojciech Zieba
- Llewellyn Morkel
- Denisa Diaconescu
- Matteo Lisotto
- Mario Apra
- Trantorian
- Charpa
- Chris
- Antiyro
- Aneeque Safdar
- Cchudant
- Rian Hughes
------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **November 7, 2024**, at **11:00 AM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.