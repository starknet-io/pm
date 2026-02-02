# Starknet All Core Devs Meeting #22
## Meeting Details

- **Date & Time:** Thursday, March 13, 2025, 12:00-12:30 PM UTC
- **Duration:** 30 minutes
- **YouTube:** [Part 1](https://youtube.com/live/fCIikziOhDg) | [Part 2](https://www.youtube.com/watch?v=D-RrtbUL87s)
- **Agenda:** https://github.com/starknet-io/pm/issues/12
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Notes

The meeting began with Aayush Giri welcoming participants and noting that Leonardo and Ariel from Starkware would not be joining, but had provided updates to be shared on their behalf. The meeting was split into two recordings due to technical issues.

### 1. Starknet v0.13.5 Upgrade Status

Aayush shared Leonardo's update on the v0.13.5 upgrade:
- v0.13.5 is currently live on Sepolia integration
- Testnet upgrade is planned for March 17th
- Mainnet, which is currently on v0.13.3, will be upgraded to both v0.13.4 and v0.13.5 on the same day, tentatively scheduled for March 24th
- A sequencer bug was identified on the v0.13.4 testnet that led to wrong estimation, but this has been resolved
- v0.13.5 includes two key additions:
  1. A whitelisted list of accounts are now compatible with transaction version 3
  2. Updated pricing of the deploy syscall

### 2. Client Implementation Updates

[Krisztian Kovacs](https://github.com/kkovaacs) reported on Pathfinder's progress:
- Released version 0.16.2 the previous day with support for Starknet v0.13.5
- Fixed several JSON RPC issues and a bug that might have caused users to think Pathfinder wasn't properly updating L1 confirmed state
- Clarified a Gateway bug where the estimation was correct but the Gateway overestimated resources needed to execute transactions, particularly affecting simple token transfers
- The issue has been resolved on the Gateway side

[Rodrigo Pino](https://github.com/rodrodros) provided an update on Juno:
- Released version 0.14.0 with support for Starknet v0.13.5
- Implemented and tested all WebSocket functionality
- Currently investigating a non-deterministic bug at the interface between Rust and Go components
- Despite this issue, the overall implementation is working well, and they plan to release a hotfix once the bug is resolved

[Nathan](https://github.com/antiyro) from Kasar Labs discussed their approach to SNOS versioning constraints:
- Facing challenges with Blockifier versions as SNOS isn't updating its version and they need SNOS for appchains
- Planning to deploy Quaza Gen on v0.13.3 first before the end of March to test if they can update Blockifier while maintaining compatibility
- Have confirmed their branch with the latest core can produce and sync blocks correctly
- Working to ensure full node support while keeping pace with core developments

### 3. Cairo Native Progress

Aayush shared Ariel's update on Cairo Native progress:
- Native v0.3.4 refactors both arrays and dictionary handling
- Currently in the process of replay testing
- If no new findings emerge, they plan to enable it on testnet for all contracts

### 4. RPC Compatibility Challenges

[Rian Hughes](https://github.com/rianhughes) discussed challenges with execution resources between RPC versions:
- RPC v6 and v7 require a non-zero "steps" field in execution resources
- This field is no longer used in RPC v8, and Blockifier no longer populates it
- Clients are typically returning a zero value, which technically violates the v6/v7 spec
- Unclear what the best approach is, as clients can't easily determine the appropriate values

The discussion also touched on L1 status update recovery issues following the Ethereum Sepolia Pectra hardfork, though details were limited.

### 5. P2P and Consensus Implementation

[Krisztian Kovacs](https://github.com/kkovaacs) raised concerns about P2P protocol safeguards:
- Noted that when manually configuring trusted peers (peers added directly, not via Kademlia discovery), it's easy to accidentally add nodes from different chains or networks
- Suggested adding chain ID verification to other parts of the protocol stack, not just discovery, to avoid hard-to-diagnose problems during configuration
- Emphasized this would be particularly helpful for testing environments

[Shahak Shama](https://github.com/ShahakShama) responded:
- The end goal is not to use manually specified peer lists
- Expressed hesitation about adding features solely for testing purposes
- Suggested using different topics for testing purposes, but was open to considering the approach if it proved necessary

[Adi Seredinschi](https://github.com/adizere) provided an update on Malachite integration:
- Two major areas of progress:
  1. Interoperability tests between Papyrus and a Malachite-based mock sequencer
  2. Integration efforts with full node clients (Madara and Pathfinder)
- Recent progress in interoperability, with Papyrus-based sequencer now able to communicate with Malachite-based sequencer
- Nodes disconnect when trying to exchange blocks, still investigating the cause
- Working with Papyrus team to gather logs and cross-validate with Malachite logs
- Received substantial feedback on the low-level core API (core consensus API) from client teams
- Developing ADRs (Architecture Decision Records) to clarify the application-consensus interface
- Explained the interface as a state machine where inputs and outputs must follow a specific grammar or sequencing of steps

### 6. L1 Data Extraction Tool

With Leonardo absent, the moderator asked if anyone had thoughts on the L1 data extraction initiative and its priority relative to other work. No participant provided input on this topic.

### 7. AOB (Any Other Business)

No additional topics were raised.

-----
### Attendees

- Aayush Giri (Moderator)
- Shahak Shama | Starkware
- Krisztian | Equilibrium
- Vaclav | EQ
- Rodrigo Pino
- Wojciech Zieba
- Daniil Ankushin
- Leonardo Lerer (absent but provided updates)
- JB | Kasar
- Chris
- Abel | EQ
- Nathan | Kasar
- Rian
- cchudant (Kasarlabs)
- Adi Seredinschi @ Informal

------------
Note: These Starknet All Core Devs Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **March 27, 2025**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.