# Starknet All Core Devs Meeting 
## Meeting Details

- **Date & Time:** [Thursday, November 21, 2024, 12:00-12:30 PM UTC](https://www.timeanddate.com/worldclock/converter.html?iso=20241121T120000&p1=1440&p2=37&p3=136&p4=237&p5=923&p6=204&p7=671&p8=16&p9=41&p10=107&p11=28&p12=438)
- **Duration:** 30 minutes
- **YouTube:** [https://www.youtube.com/watch?v=QeSYeCxswfo&list=PLMXIoXErTTYW7_3FjybBzJXhfZwvSchPa](https://www.youtube.com/watch?v=QeSYeCxswfo&list=PLMXIoXErTTYW7_3FjybBzJXhfZwvSchPa)
- **Agenda:** [#15](https://github.com/starknet-io/pm/issues/4)
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Notes

### 1. Starknet v0.13.3 & v0.13.4 readiness updates:

- Starkware (update by [Leonardo Lerer](https://github.com/leo-starkware)): Testnet currently running v0.13.3 which already has **stateless compression**. Mainnet upgrade is scheduled for **Wednesday, November 27**. Teams running Juno nodes should plan to upgrade. For **v0.13.4**, Cairo native integration progressing well, targeting end of year/beginning of next year for integration.

### 2. Client implementation updates:

- Pathfinder (update by [Krisztian Kovacs](https://github.com/kkovaacs)): Planning release with preliminary support for new **JSON RPC** this week.

- Juno (update by [Kirill Paltsev](https://github.com/kirugan)): Merged `starknet_getMessageStatus`. `starknet_getStorageProof` nearly complete with minor review changes pending. **WebSocket** implementation progressing with bug fixes in existing methods. Most implementations complete and are pending review.

- Kasar Labs (update by [Trantorian](https://github.com/Trantorian1)): Completed majority of **RPC 0.8** implementation. **WebSocket** implementation progressing with 1-2 methods remaining. Timeline aligned with v0.13.4 release.

### 3. P2P Specification Updates:

[Shahak Shama](https://github.com/ShahakShama) recapped the network architecture proposal from the [previous call](https://github.com/starknet-io/pm/blob/0db2e0319f38e94ab486fc85346cb25b228320d5/AllCoreDevs-Full-Nodes-Meetings/call_014.md) for absent teams: The plan involves implementing three separate **Kademlia networks** - state sync, consensus, and mempool transaction propagation. Nodes will connect to relevant networks based on their roles, with dual-role nodes creating separate identities for each network. The implementation will use gossipsub initially with flexibility to change if needed. This modular approach allows for better consensus network management while keeping state sync layer open to all nodes. For complete details of the architecture and implementation plan, teams can refer to the [previous meeting notes](https://github.com/starknet-io/pm/blob/0db2e0319f38e94ab486fc85346cb25b228320d5/AllCoreDevs-Full-Nodes-Meetings/call_014.md?plain=1#L40).

### 4. Consensus implementation progress:

(update by [Aneeque Safdar](https://github.com/IronGauntlets)): Consensus implementation status remains similar to previous update. Tendermint implementation is complete. Currently focused on testing with approximately half of the state verification tests implemented. Some bugs identified requiring fixes. WebSocket implementation taking current priority. Recent meeting with Informal Systems highlighted discussion around "[Amnesia attack](https://messari.io/report/long-range-attack)" (a variant of Long Range attacks in consensus networks), with team exploring potential mitigations.

### 5. P2P integration progress:

- Juno/Pathfinder Sync Status (update by [Kirill Paltsev](https://github.com/kirugan)): Fixed compatibility issues between clients. Juno successfully syncing from Pathfinder. Bidirectional sync partially working, with remaining issue in pre-authored style commitments for block headers during Pathfinder-from-Juno sync.

- P2P Testnet Discussion:
  - Discussion to start with private network (whitelisted nodes)
  - Need to define success criteria for public launch
  - Considering malicious peer testing implementation
  - [Mikolaj Barwicki](https://github.com/stranger80) to draft criteria document

### 6. Forward Compatibility Approaches:

Discussion initiated by [Aneeque Safdar](https://github.com/IronGauntlets) about client approaches to network upgrades and database handling:

- Pathfinder (update by [Krisztian Kovacs](https://github.com/kkovaacs)): No explicit version checks, but implements strict Feeder Gateway response validation. Does not allow unknown JSON properties in responses.  While technically forward compatible, most Starknet upgrades require at least minor code changes due to schema changes or fee computation updates.

- Juno ([Aneeque Safdar](https://github.com/IronGauntlets)): Currently implements version compatibility checks to prevent DB corruption. Team is reconsidering this approach given less frequent Starknet releases. Current implementation in Golang allows new JSON fields to be ignored during unmarshaling, which could lead to missing data.

- Madara (update by [Trantorian](https://github.com/Trantorian1)): Not currently forward compatible due to potential DB changes between versions. Developing solution using non-synchronizing Madara Feeder Gateway for rapid DB updates. This allows local synchronization while trusting state root, enabling faster transition between database formats without requiring full resync.

Teams discussed various approaches to handling schema changes and database migrations, with focus on balancing safety with operator convenience.

### 7. P2P Network Testing:

- [Wojciech Zieba](https://github.com/wojciechos) announced new [P2P Testing Dashboard](https://starknet-p2p-testing-dashboard.onrender.com/)
- Dashboard tracks cross-client testing progress
- Currently focused on Juno implementation, plans to add other clients
- Using Madara sequencer for testing environment

-----
### Attendees

- Aayush Giri (Moderator)	
- Wojciech Zieba
- Trantorian
- Leonardo Lerer
- Mikolaj Barwicki
- Matteo Lisotto
- Vaclav
- Kirill Paltsev
- Krisztian Kovacs
- Chris 
- penovicp
- Aneeque Safdar
- Mohit 
- Jean-Baptiste Caron
- Shahak Shama 
- Pawel Nowosielski
- Ariel Elperin
- Daniil Ankushin
- Tomasz Stanczak

------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **December 5, 2024**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.