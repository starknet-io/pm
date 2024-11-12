# Starknet All Core Devs Meeting 
## Meeting Details

- **Date & Time:** [Thursday, November 7, 2024, 12:00-12:30 AM UTC](https://www.timeanddate.com/worldclock/converter.html?iso=20241107T120000&p1=1440&p2=37&p3=136&p4=237&p5=923&p6=204&p7=671&p8=16&p9=41&p10=107&p11=28&p12=438)
- **Duration:** 30 minutes
- **YouTube:** [https://www.youtube.com/watch?v=6W8C9XzqKAw&list=PLMXIoXErTTYW7_3FjybBzJXhfZwvSchPa](https://www.youtube.com/watch?v=6W8C9XzqKAw&list=PLMXIoXErTTYW7_3FjybBzJXhfZwvSchPa)
- **Agenda:** [https://github.com/starknet-io/pm/issues/3](https://github.com/starknet-io/pm/issues/3)
- **Moderator:** [Leonardo Lerer](https://github.com/leo-starkware)


## Meeting Notes

### 1. Starknet v0.13.3 readiness updates:

- Starkware (update by [Leonardo Lerer](https://github.com/leo-starkware)): Announced revised roadmap with v0.13.3 integration planned for coming weeks. This version will include **stateless compression** of state diffs to address rising block prices on mainnet and modified fee computation for data guards (which will simply be a number in the header of the blocks which is computed differently). No breaking changes required for client teams. **Version 0.13.4**, planned for **mid-January** integration, will contain **RPC 0.8**, **L2 gas resource**, and **stateful compression** which compresses keys and addresses using a state-stored counter. It's not yet clear if v0.13.4 will include **Cairo native** integration, as this depends on integration progress. The delay in Cairo native integration is primarily due to integration work complexity and some issues in the native compiler, particularly around error handling expected by the blockifier. Testing time is also a critical factor in this decision.


- Kasar Labs(update by [Antiyro](https://github.com/antiyro)): Team has completed most of **RPC 0.8** integration. **WebSocket** implementation is running well with only 1-2 methods remaining. Awaiting confirmation about RPC methods implementation timeline, which was clarified to be part of **0.13.4**.


- Nethermind (update by [Kirill](https://github.com/kirugan)): Team has merged one RPC method to main, with two others in review. The [`starknet_getStorageProof`](https://github.com/NethermindEth/juno/issues/2180) is almost complete with only minor review changes pending. The [`starknet_getMessageStatus`](https://github.com/NethermindEth/juno/pull/2184) is in final stages. For **WebSocket** implementations, almost all methods are in PRs except the [`unsubscribe` ](https://github.com/NethermindEth/juno/issues/2177#:~:text=starknet_unsubscribe) method.


- Pathfinder (update by [Krisztian](https://github.com/kkovaacs)): All **RPC 0.8** methods are supported with almost all PRs merged to main. Planning a pre-release with preliminary preview support for the new **JSON RPC API** to facilitate testing by JSON RPC client teams. Current version can sync from mainnet but returns 0 for **L2 gas** in the new JSON RPC version. The [`starknet_getCompiledCasm`](https://github.com/eqlabs/pathfinder/issues/2329) implementation has been merged to main, pending additional testing before release.


### 2. P2P integration progress:

- Kasar Labs(update by [Antiyro](https://github.com/antiyro)): Started integration this week with block syncing expected by end of week.


- Pathfinder (update by [Krisztian](https://github.com/kkovaacs)): Fixed several problems that prevented syncing between **Juno** and **Pathfinder**. Juno can now sync from Pathfinder. Bidirectional sync partially working, with remaining issue being pre-authored style commitments in block headers when Pathfinder syncs from Juno.


- Nethermind (update by [Wojciech](https://github.com/wojciechos)): Implemented testing approach using **Madara** sequencer with custom network and **P2P** block broadcasting. Currently working only for Juno as its P2P sync doesn't require **L1**. Unable to add Pathfinder nodes due to L1 requirement, though Pathfinder team confirmed possibility of disabling checkpoint sync. Team maintains manual compatibility tracking via table in **Starknet P2P** test repository. Using **Kurtosis** framework for test network deployment.


### 3. P2P Specification Updates:

[Shahak](https://github.com/ShahakShama) presented two major developments:
1. **Version Management** in P2P Specs Repository:
  The plan is to create a branch for **0.1.0** for the specs, while the main branch continues development with breaking changes. The version branch (0.1.0) shouldn't change much - only for bug fixes or small fixes that don't require full nodes to update all specs. For example, adding **L2 gas** when **0.13.4** releases could be added to the version branch with a rename to **0.2.0** due to it being a breaking change. This allows teams to continue working on the **0.1.0** branch without needing constant updates while main branch development continues uninterrupted. Discussion clarified that adding new methods isn't breaking as it doesn't affect running nodes, but renaming fields or changing field types would be breaking changes.

2. **Network Architecture**:
  Announced implementation of three separate [**Kademlia networks**](https://en.wikipedia.org/wiki/Kademlia) for different protocol families: **state sync**, **consensus**, and **mempool** (for sharing transactions between mempools). A node that follows state (without creating new blocks) will connect to one Kademlia network, while a consensus node will connect to a different network. For nodes implementing consensus that also need state sync, they will create two different node identities inside the executable, with each identity connecting to different networks. Currently planning to use **gossip sub** mechanism as it's already implemented, with possibility to switch to another publish/subscribe protocol if gossipsub becomes a bottleneck. This separation provides advantages in managing the consensus network more tightly and adds modularity - potentially allowing future implementations of standalone consensus nodes and full nodes that interact with each other. The state sync layer will be open to all types of nodes, while the consensus layer will be restricted to nodes participating in consensus.

### 4. Consensus implementation progress:
No updates provided as [Denisa](https://github.com/denisadiaconescu) was not present in the call.


### 5. Staking updates - Stage 2 preparations:
Topic postponed due to time constraints. [Leonardo Lerer](https://github.com/leo-starkware) suggested continuing discussion in full node collab channel.


-----
### Attendees

- Leonardo Lerer
- Antiyro
- Shahak Shama
- Kristzian Kovacs
- Llewellyn Morkel
- Chris
- Kirill Paltsev
- Wojciech Zieba
- Stefan
- Trantorian
- Vaclav
- Matteo Lisotto
- Abel E
- Chara
- Cchudant
- Fabijan Corak
- Rian
- Aneeque
- Amanusk
- Ariel Elperin


------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for **Thursday, November 21, 2024**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.