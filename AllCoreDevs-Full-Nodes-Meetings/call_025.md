# Starknet All Core Devs Meeting #25
## Meeting Details

- **Date & Time:** Thursday, April 24, 2025, 11:00-11:30 AM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/live/1jTe8vL72II
- **Agenda:** https://github.com/starknet-io/pm/issues/15
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Notes

### 1. Mainnet v0.13.5 Operation Status

Leonardo Lerer reported that v0.13.5 operations are running smoothly with no significant issues to report. He noted that most of the major changes were included in v0.13.4, with v0.13.5 containing only minor additions on top. The network has been stable over the past three weeks of production deployment. Leonardo clarified that previous gateway investigation issues mentioned in earlier calls related to upgrade process issues from about a month ago have all been resolved. Overall, the team's energy and focus has shifted primarily to v0.14.0 development.

### 2. Starknet v0.14.0 Preview

Leonardo provided significant updates on the upcoming v0.14.0 release, which represents a major architectural change for Starknet. He announced a tentative integration date set for Tuesday (April 29th), with plans to confirm by Sunday as they approach the date. The biggest architectural change will be the introduction of multiple sequencers, most likely four sequencers in production, fundamentally changing how the network operates.

A critical change involves splitting URLs between the feeder gateway and gateway. The gateway URL will remain unchanged, but the feeder will be located at a different URL. This requires client teams to refactor their codebases that currently use the same prefix base URL and add routes after that. Leonardo emphasized this should be a small refactor for client teams but is necessary for the new architecture. The gateway's post method for adding transactions will go to a load balancer between the four sequencer nodes, rather than through a centralized component where the feeder database lives. This enables direct communication with sequencers without going through centralized components first, making sense for the future vision where there will be no feeder gateway.

Significant changes to pending block behavior were outlined. Currently, pending blocks are always increasing, constructed by the sequencer and published when intermediate runs end, with the prefix guaranteed to be in the final block. In v0.14.0, pending blocks will be the latest block that consensus agreed upon, either without all commitments (transaction, event, and state commitments) if queried before processing services finish, or the latest block if those services have completed. This eliminates the current behavior where querying pending might return different transaction counts at the same height.

Leonardo confirmed they will not be implementing RPC 0.9 to remove pending objects in this version, maintaining support for pending objects to avoid overloading an already substantial release. He hopes that changes required by nodes for v0.14.0 will be minimal except for the URL splitting, enabling a relatively fast release so other ecosystem players affected by this version can have a node version to work with.

### 3. Cairo Native Implementation Progress

Since Ariel Elperin was not present, Leonardo provided updates on Cairo Native progress. He reported that the technical issues around deploying and using shadow environments for testing that were mentioned two weeks ago have been resolved. Rather than providing specific performance metrics, Leonardo announced that Cairo Native will likely be activated on integration very soon, followed by testnet deployment approximately one week after integration testing begins. This approach should provide better performance numbers than preliminary estimates, giving more concrete data about the implementation's effectiveness.

### 4. Client Team Updates

**Pathfinder** (Krisztian Kovacs):
- Malachite integration progressing on multiple fronts, including both integrating Malachite and implementing proposal validation within Pathfinder, though still in early stages with no concrete results yet
- Released hot fix last week addressing several bugs, including a particularly problematic issue where some events were missing from `starknet_getEvents` results - all users advised to upgrade to latest release
- Working on staking v2 attestation tool with planned release this week or early next week, adding compatibility with clear signing API implemented by Juno team, and fixing several bugs since last release
- Ongoing discussion in node collab channel about adding meaningful defaults for well-known Starknet contract addresses to reduce manual configuration requirements, though mainnet contract addresses not yet available

**Juno** (Rodrigo Pino):
- Resolved estimate_fee bug with pre-release version 14.4 now available
- Dynamic timeout implementation working well in practice, increasing default timeout from 5 to 10 seconds, which may reduce traffic to sequencer
- Staking v2 validator implementation progressing with bug fixes completed (including resolving difficult-to-find issue with wrong address usage) and focus shifting to UX improvements including configuration files, environment variables, and flags
- Adding checks to ensure attestation tool is used with synced node, as many users encounter issues when nodes aren't synchronized
- Block building capability now available as experimental feature in latest pre-release, with documentation provided for testing (though not yet considered reliable feature)

**Kasar Labs** (Representative speaking for Nathan):
- Continuing to merge changes from development network into Madara core, including v0.14.0 RPC updates for mainnet
- Changes include sync pipeline improvements focused on performance updates and efficiency gains
- Slowly integrating these improvements back from DevNet into core Madara codebase, resulting in better overall performance

### 5. Staking v2 Implementation Status

The moderator provided an update based on information from Nathan, announcing that the mainnet launch has been rescheduled for May 13th, revised from the previously mentioned May 6th target date. The new version is already live on testnet for testing and issue reporting. The final timeline will be determined after an internal meeting. In preparation for the upgrade, a second validator call has been scheduled for Tuesday, April 29th, during which the team will discuss upcoming changes and explain how to prepare for the transition.

### 6. Malachite Consensus Integration

Adi Seredinschi was not present - agenda item skipped.

-----
### Attendees

- Aayush Giri (Moderator)
- Leonardo Lerer
- Krisztian | Equilibrium  
- Vaclav | EQ
- Rodrigo | Neth
- Trantorian (Kasarlabs)
- Abel | EQ
- Fireflies.ai Notetaker
- Itay Dror
- rian
- Eitan Moed
- JB | Kasar
- Wojciech Zieba
- cchudant

------------
Note: These Starknet All Core Devs Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **May 8, 2025**, at **11:00 AM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.
