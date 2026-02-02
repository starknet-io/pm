# Starknet All Core Devs Meeting 
## Meeting Details

- **Date & Time:** Thursday, January 30, 2025, 12:00-12:30 PM UTC
- **Duration:** 30 minutes
- **YouTube:** https://youtube.com/live/JQoMZTtZbJU
- **Agenda:** https://github.com/starknet-io/pm/issues/9
- **Moderator:** Aayush Giri

## Meeting Notes

 Welcoming participants and setting the stage for discussions following an eventful week that included the Core Dev Summit.

### 1. Core Dev Summit Outcomes

[Ilia](https://github.com/iliav-starkware) provided a concise overview of the two-day summit's key outcomes. The first day focused heavily on P2P implementation strategy, with a significant decision to couple P2P spec releases with RPC version releases. This coupling aims to prevent complications for client teams who previously struggled with determining which P2P spec version to sync against. The discussions also centered on establishing a reasonable working plan for consensus integration, resulting in improved synchronization between Informal Systems and client teams.

The second day featured research-oriented discussions about unprovable blocks, though detailed technical aspects were reserved for future specialized forums. A notable clarification emerged during the discussion: the team decided to prioritize validation over P2P sync and mempool sync, specifically in the context of consensus integration. This represents a strategic shift in implementation priorities moving forward.

### 2. Blockifier Integration Progress

[Ariel Elperin](https://github.com/ArielElperin) provided updates on the blockifier integration, focusing on upcoming developments in RC1. The team is preparing a PR for state diffs implementation using the executor, though these changes will remain optional rather than mandatory. Ariel emphasized that while the current execution methods remain valid, the executor provides a more convenient approach.

A significant update involves the handling of L1 gas for tracing, addressing current limitations where internal calls only have L2 gas. Ariel referenced an ongoing PR from Krisztian to the blockifier that addresses these issues.

RC1 will also introduce significant improvements to the build process, particularly regarding Cairo native integration. The team has opted for a static linking solution that eliminates the need for a separate runtime file, resulting in approximately 20x smaller contract artifacts. While this change brings substantial benefits in terms of build simplicity and artifact size, Ariel noted the importance of careful API management for built-ins in Cairo native when dealing with version upgrades.

### 3. Client Implementation Updates

[Krisztian](https://github.com/kkovaacs) reported on Pathfinder's progress, highlighting their work on gas cost calculations for inner calls within transaction traces. The team has begun implementing the fee estimation algorithm and adding support for JSON-RPC 0.6. He emphasized the challenges in validating L1 gas cost calculations due to the absence of canonical transaction traces from the feeder gateway or sequencer.


[Rodrigo Pino](https://github.com/rodiazet) updated on Juno's progress, noting they have begun blockifier integration with an open PR under review. The team has resolved previous system contract issues by adding it to the database. Current focus areas include reviewing the JSON-RPC 0.6 package and updating WebSocket implementations to align with the latest specifications.

The Kasar Labs representative [Nathan](https://github.com/antiyro) shared significant progress in their P2P implementation, reporting successful block syncing and propagation through the P2P network. While refinements are still needed, they expect to pass the Nethermind P2P tests soon, ensuring compatibility with other clients. Regarding Madara exploration, they reported being close to completing work with Cairo native for v0.13.4 and have begun collaboration with Informal Systems on consensus integration. Their immediate goal is to deploy their own network, targeted for next month, which will serve as a development and testing environment. This network is described as "Madara on steroids" and will function as a staging version for implementing core functionality. The team expressed their intention to leverage both P2P and consensus capabilities in this network, similar to what is planned for the next version of Starknet, potentially serving as a model for decentralized network implementation.

### 4. Technical Discussions

Regarding gas estimation challenges, [Ariel Elperin](https://github.com/ArielElperin) reiterated the ongoing complexity introduced by gas redeposits, which complicates the standard approach of running with infinite gas and calculating the difference. This issue continues to require careful consideration in implementation strategies.

### 5. Implementation Timeline

[Leonardo Lerer](https://github.com/leo-starkware) provided a brief timeline update for version 0.13.4, indicating that testnet deployment is expected by mid-February, followed by mainnet deployment approximately one month later. Client teams have been informed through relevant forums and are preparing accordingly.

### 6. Informal Systems Update

[Romain](https://github.com/romac) from Informal Systems announced plans for a devnet to test integration between Malachite and Papyrus in the coming weeks. The immediate focus is on aligning Protobuf definitions to ensure proper interoperability. The team expressed readiness to assist with Malachite integrations as client teams reach out with questions.

### 7. Next Meeting Schedule

The next meeting is scheduled for February 13, 2025, at 12:00 PM UTC.

-----
### Attendees


- Aayush Giri
- Shahak
- Eitan 
- Krisztian
- Nathan
- JB
- Leonardo Lerer
- Daniil Arkushin
- Trantorian
- Mohit
- Abel E
- Matteo
- Vaclav
- rian
- cchudant
- denisadiaconescu
- Chris
- Romain
- amanusk
- Nathan
- Ariel Elperin


------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for Wednesday, **February 13, 2025**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.