# Starknet All Core Devs Meeting #20
## Meeting Details

- **Date & Time:** Thursday, February 13, 2025, 12:00-12:30 PM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/watch?v=QHUOB6oHoJo&t=2s
- **Agenda:** https://github.com/starknet-io/pm/issues/10
- **Moderator:** Aayush Giri

## Meeting Notes

The meeting opened with Aayush Giri welcoming participants and emphasizing the importance of the session as teams approach the critical February 20th deadline for Starknet v0.13.4 integration. The focus would be on readiness updates and potential blockers for client teams.

### 1. Starknet v0.13.4 Integration Updates

[Leonardo Lerer](https://github.com/leo-starkware) provided an update on the v0.13.4 timeline, reporting that progress has been good and all teams are expected to meet the February 20th deadline. Following client releases, approximately one week will be required for integration, allowing RPC providers and ecosystem participants to upgrade their nodes. Leonardo anticipates testnet deployment toward the end of February, though no fixed date has been established yet.

Discussion moved to the proposed changes in entry point offsets for deprecated classes. [Ariel Elperin](https://github.com/ArielElp) explained that this involved a minor fix related to classes that are no longer declarable. While included in RC3, this change is entirely unrelated to the v0.13.4 upgrade and would create additional integration work. After discussion with client teams, they mutually agreed to postpone implementation rather than include it in the current upgrade cycle.

### 2. Blockifier Integration Progress

[Ariel Elperin](https://github.com/ArielElp) shared that the blockifier team had faced some challenges with Cairo native that delayed the next release candidate. However, a new Cairo native version was published the previous day, resolving all known issues. Ariel expects to release a new blockifier RC early next week based on this update, with APIs for native integration now considered stable.

Regarding state diffs, Ariel confirmed they have been merged for the executor. The blockifier includes all necessary code to handle Cairo native, though the team may delay enabling native in the sequencer pending an important optimization. The optimization relates to array handling in Cairo native, where arrays are currently cloned on write operations. The team is working on a solution that would prevent this cloning, significantly improving performance. Ariel estimated this would take approximately a week to implement but emphasized it would not block the blockifier release candidate or the v0.13.4 upgrade process.

Looking forward, Ariel noted that while there may be internal updates to Cairo native, these changes should be transparent to teams using the blockifier dependency. The team can bump the Cairo native version without affecting external implementations.

### 3. Client Implementation Updates

[Krisztian Kovacs](https://github.com/kkovaacs) reported on Pathfinder's progress with fee estimation implementation. The team continues to work on the binary search algorithm, discovering nuances about when binary search is necessary and identifying edge cases where it can be optimized or avoided. Krisztian mentioned they expect to have an initial implementation ready by early next week, followed by a beta release of Pathfinder that will be feature-complete for Starknet v0.13.4 and JSON-RPC 0.8. Assuming everything proceeds as planned, they intend to release a stable version by the February 20th deadline.

For Juno, [Rodrigo Pino](https://github.com/rodrigo-pc) shared that they have integrated the latest blockifier release candidate and are currently implementing fee estimation functionality. The team has completed a significant refactoring of their RPC package to improve maintainability for version management. Rodrigo noted they have been focusing on testing and addressing minor bugs in recent weeks.

[Nathan](https://github.com/antiyro) from Kasar Labs discussed their approach to codebase stabilization. The team has encountered challenges with SNOS versioning for app chains, as SNOS uses blockifier and requires updating to the next version. Ongoing discussions are focused on how to merge the new blockifier update without compromising app chain deployments. Nathan explained that while they work on a solution, any v0.13.4-related implementations will be developed in a separate branch for future integration.

Regarding P2P implementation, Nathan reported continued testing and improvements, with plans to integrate their implementation into Nethermind's testing suite. Elliot, speaking about consensus work, described their focus on message streaming infrastructure to ensure the ability to receive and process messages as they arrive. Elliot raised a question about whether streaming messages for consensus are processed one proposal at a time, but as the relevant team members were not present, the discussion was deferred to the Slack channel.

### 4. Malachite Integration Updates

[Adi](https://github.com/adizere) provided an update on three key aspects of Malachite integration. For devnet testing, the team is working to recreate tests using existing scripts in Papyrus, with the goal of deploying a local testnet capable of communication between Papyrus and a Malachite-based sequencer. Adi mentioned they had encountered edge cases during setup but had a productive discussion with Matan that morning that unblocked their progress.

Regarding Protobuf definitions, Adi reported alignment between the Starkware and Malachite teams. However, [Trantorian](https://github.com/Trantorian1) raised concerns that the consensus.proto specifications on the main branch were still marked as work-in-progress, contrary to Adi's understanding that they were considered stable. Adi committed to following up on this discrepancy in the integration channel.

On integration support for client teams, Adi noted they hadn't received specific requests yet, which they interpreted as teams focusing on other priorities. The Malachite team has conducted internal experiments to anticipate potential issues, particularly around low-level APIs that will be used by Madara and Pathfinder. Specific edge cases include handling multiple proposers for the same height and rounds, as well as signature aggregation handling that may be relevant depending on implementation details.

### 5. AOB (Any Other Business)

No additional topics were raised by participants.

### Next Steps

The meeting concluded with a reminder from Aayush that client teams should aim to complete their implementation work by the February 20th deadline. The blockifier team will release the next RC early next week, with testnet deployment expected by the end of February. Technical discussions will continue in the dedicated Slack channel.

-----
### Attendees

- Rodrigo Pino
- Aayush Giri
- Krisztian | Equilibrium
- Daniil Ankushin
- Adi Seredinschi | Informal
Leonardo Lerer
- Ariel Elperin
- Vaclav | EQ
- Trantorian | Kasarlabs
- Abel E | EQ
- Chris
- Rian
- Nathan | Kasar
- Wojciech Zieba
- Nikša (EQ)
- Eitan Moed

------------
Note: These Starknet Full Node Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **February 27, 2025**, at **12:00 PM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.