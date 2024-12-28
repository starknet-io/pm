# Starknet All Core Devs Meeting 
## Meeting Details

- **Date & Time:** [Thursday, December 19, 2024, 12:00-12:30 PM UTC](https://www.timeanddate.com/worldclock/converter.html?iso=20241219T120000&p1=1440&p2=37&p3=136&p4=237&p5=923&p6=204&p7=671&p8=16&p9=41&p10=107&p11=28&p12=438)
- **Duration:** 30 minutes
- **YouTube:** [https://www.youtube.com/watch?v=TOFQG5ic8UA&list=PLMXIoXErTTYW7_3FjybBzJXhfZwvSchPa&index=1](https://www.youtube.com/watch?v=TOFQG5ic8UA&list=PLMXIoXErTTYW7_3FjybBzJXhfZwvSchPa&index=1)
- **Agenda:** [https://github.com/starknet-io/pm/issues/7](https://github.com/starknet-io/pm/issues/7)
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Notes

### 1. Sequencer Development and Infrastructure Transition

[Leonardo Lerer](https://github.com/leo-starkware) discussed the next-generation sequencer and MOL integration plans. The team is still finalizing internal discussions on protocol features for the next phase, with no immediate RPC upgrade planned. Existing user interactions are expected to remain seamless throughout the transition.

[Jorik](https://github.com/JorikSchellekens) raised critical points about infrastructure transition planning, emphasizing the need for proactive coordination with RPC provider teams. He stressed that teams must be prepared with direct connections to different sequencers and have proper access to the upcoming mempool implementations. Jorik also highlighted that Starkware needs to maintain at least one node feeding the Feeder Gateway during the initial transition phase to ensure service continuity, as there are currently no immediate plans for Feeder Gateway deprecation.

[Jorik](https://github.com/JorikSchellekens) then initiated a broader discussion about the relationship between execution and consensus layers, seeking feedback from all teams about potential development paths. He raised fundamental questions about whether there should be clear separations between these components and whether teams should pursue client diversity through interchangeable implementations or continue with monolithic approaches. The key question posed was whether implementations like Juno should interface with Malachite earlier, or if teams should continue with their current language-specific implementations.

[Shahak Shama](https://github.com/ShahakShama) responded to these architectural questions by explaining their planned approach to P2P network splitting. He described a system where nodes can discover and connect to different types of nodes (sync nodes, consensus nodes, or mempool nodes) independently. This design enables the possibility of running separate implementations for sync nodes and consensus nodes while allowing them to interact. While this modular approach is possible, Shahak noted that the decision of whether nodes should implement this separation remains up to individual teams for now. He suggested that a formal protocol for communication between execution and consensus nodes might be developed in the future, though the immediate focus is on getting the network operational.

This sparked a detailed technical discussion about modularity levels and implementation boundaries. Questions arose about whether modularity should be restricted by programming language or if cross-language communication should be supported as long as proper message encoding protocols are in place. The teams acknowledged that supporting language-agnostic modularity would require more extensive specification work, particularly around message structures and APIs needed for interaction between execution and consensus clients. Participants noted that while they could draw inspiration from Ethereum's execution-consensus separation model, such decisions need to be made early to avoid significant engineering rework later.

### 2. Client Implementation Updates

- [Krisztian](https://github.com/kkovaacs) reported that Pathfinder's JSON RPC implementation is now complete, though the team is actively seeking meaningful feedback from users, particularly regarding the WebSocket API and new method implementations.

- [Kirill Paltsev](https://github.com/kirugan) updated that Juno's WebSocket implementation continues with two pending PRs. Despite discovering a bug in one implementation yesterday, the team remains confident about merging both PRs before year-end.

- Kasar Labs has made significant progress on their P2P implementation, though detailed updates were unavailable due to [Antiyro](https://github.com/antiyro)'s absence from the meeting.

### 3. Consensus Implementation and SOS Blocks

[Denisa](https://github.com/denisadiaconescu) shared that discussions with Leo regarding SOS blocks implementation are ongoing. The team is currently waiting for certain implementation components before moving forward, with progress expected to resume in early 2025. Regarding Amnesia attack mitigation, a recent meeting with Informal Systems focused primarily on implementation synchronization rather than research aspects. The team plans to continue research discussions in early 2025.

### 4. Malachite Integration and Documentation

[Adi Seredinschi](https://github.com/adizere) announced that the Malachite repository has been made open source ahead of the original schedule. The team accelerated their release timeline and has completed comprehensive documentation for the implementation. The documentation includes detailed architecture documentation describing how to build general-purpose applications on top of the core Malachite library (the Tendermint library). It also covers additional documentation for building on the outer layers of the engine when using higher-level primitives, with specific assumptions about using Tokio channels, Tokio runtime, and other components.

Regarding integration progress, initial discussions with the Juno team have begun to explore implementation feasibility. Integration with Rust-based clients like Pathfinder and Madara appears more straightforward due to their common language base. Following initial discussions from November where repository access was provided, the team created a dedicated slack channel to maintain close communication during integration. No technical blockers have been identified, though the team needs to do initial integration work with Equilibrium and Kasar teams to identify any potential intermediary abstraction layers that might be necessary when combining codebases.

In response to integration questions, Adi confirmed the team has published to crates.io, making it easier for teams to integrate these crates into their codebases. The documentation is considered complete for the initial phase, though they expect to expand it as teams begin practical integration work. The team's focus has shifted from the research aspects of Amnesia attacks and accountability to finalizing the engineering implementation, particularly in preparing the Tendermint Rust engine for public release.

### 5. Execution and Consensus Layer Architecture

[Shahak](https://github.com/ShahakShama) presented plans for P2P network splitting, proposing separate networks for sync nodes, consensus nodes, and MLE nodes. This architecture allows implementations to connect different components flexibly, though it may require specific protocol specifications for execution-consensus communication. The discussion explored language-agnostic implementation possibilities and message specification requirements, with participants suggesting potential inspiration from Ethereum's execution-consensus separation model. Network protocol boundary considerations formed a significant part of the conversation.

### 6. Next Meeting Schedule

Due to the holiday season, the meeting scheduled for January 2, 2025, will be postponed. The next meeting will be held on January 16, 2025.

-----
### Attendees

- Aayush Giri 
- Aneeque
- Leonardo Lerer
- Kirill 
- Chris
- cchudant 
- Jorik
- amanusk
- Shahak Shama 
- Krisztian 
- Denisa Diaconescu
- Abel E 
- Llewellyn Morkel
- Matteo 
- Vaclav 
- Carmen
- Nikša 
- Rodrigo
- Itay Dror
- rian
- Ariel Elperin
- Adi Seredinschi
- Wojciech Zieba
- penovicp
- Pawel Nowosielski
- Toni Tabak
- Trantorian

------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **January 16, 2025**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.