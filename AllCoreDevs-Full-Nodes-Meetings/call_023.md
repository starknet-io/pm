# Starknet All Core Devs Meeting #23
## Meeting Details

- **Date & Time:** Thursday, March 27, 2025, 12:00-12:30 PM UTC
- **Duration:** 30 minutes
- **YouTube:** https://youtube.com/live/Z_-3LktkmCY
- **Agenda:** https://github.com/starknet-io/pm/issues/13
- **Moderator:** Aayush Giri

## Meeting Notes

The meeting began with Aayush Giri welcoming everyone and explaining that the agenda would focus on follow-up from recent deployment activities and ongoing development efforts.

### 1. Mainnet v0.13.4 and v0.13.5 Deployment Status

[Leonardo Lerer](https://github.com/leo-starkware) reported on the March 24th deployment process:
- The team upgraded from v0.13.3 to v0.13.4 on Monday, followed by v0.13.4 to v0.13.5 on Tuesday
- The first upgrade (to v0.13.4) was more significant and encountered issues that led to approximately 20 minutes of downtime
- The root cause was a breaking change in storage that required resyncing, which wasn't properly completed
- The team decided to revert to v0.13.3 temporarily and restart the upgrade process a few hours later, after which everything proceeded without issues

Regarding provider migration, Leonardo noted that most providers had been responsive in upgrading their nodes. While there were some issues, both related and unrelated to the upgrade, all have been fixed. He mentioned there are still some items to be checked in the Gateway that are under investigation.

### 2. Client Team Updates Post-Upgrade

[Rodrigo Pino](https://github.com/rodrodros) shared Juno's experience:
- RPC functionality is stable with no error reports from users
- The team encountered issues with nodes appearing to be stuck when processing large blocks due to a default timeout of 5 seconds
- They received many user complaints about this issue
- The team is implementing a dynamic timeout window that increases based on block size to better handle this in the future

[Krisztian Kovacs](https://github.com/kkovaacs) reported a similar experience with Pathfinder:
- Also encountered problems fetching large blocks due to the 5-second default request timeout
- Noted that this timeout is configurable in Pathfinder, and they hope affected parties have reconfigured their nodes accordingly
- Mentioned ongoing investigations into interesting issues with the `starknet_getEvents` calls for certain RPC providers running Pathfinder
- Some of these issues have been resolved, but the team is still working to identify the root cause for others
- Emphasized these issues are likely unrelated to the Starknet upgrade itself

[Trantorian](https://github.com/Trantorian1) provided an update from Kasar Labs:
- Development efforts are currently split between two teams:
  1. CLE Lab team working on exploration, particularly the Quaza testnet to test Madara in a production environment
  2. Moonsong handling core codebase stabilization
- Clarified that Madara does not currently support v0.13.4
- Suggested contacting Moonsong Labs for more information about implementation plans

### 3. Cairo Native Implementation Status

[Ariel Elperin](https://github.com/ArielElperin) provided an update on Cairo Native progress:
- The replay testing is still ongoing and will take a few more days to complete
- No differences have been encountered so far, making him optimistic about running v0.13.4 on testnet soon
- Performance assessment is challenging as Lambda class benchmarks didn't show significant changes
- The benchmarks didn't include specific tests targeting heavy dictionary usage, mainly focusing on replaying transactions
- Regarding production rollout, Ariel indicated it's too early to provide a definitive timeline
- If no differences are found in the replay testing, they could have it running on testnet in 1-2 weeks
- For mainnet deployment, they want extensive testing on testnet and potentially a shadow fork of mainnet
- The process will likely take at least a few months to build sufficient confidence

### 4. P2P Protocol Configuration Safeguards

Aayush referenced the previous call's discussion about P2P protocol safeguards, particularly regarding chain ID verification and misconfiguration risks with manual peer setup.

[Shahak Shama](https://github.com/ShahakShama) responded:
- Expressed no strong opinion on the subject
- Indicated willingness to add chain IDs if others thought it was valuable

[Krisztian Kovacs](https://github.com/kkovaacs) reaffirmed his support:
- Believes this addition would be helpful, particularly for debugging
- While not having a significant impact on production, it would serve as an additional safeguard to clarify misconfiguration issues

The discussion led to consensus on adding chain IDs to both protocol names and topics, with Krisztian noting that if it's added to topics, it makes sense to add it to protocol names as well.

[Nathan](https://github.com/antiyro) added context on the importance for app chains:
- For L3s and app chains sharing the gossip network with the L2 settlement layer, differentiation is critical
- Without proper identification, nodes might start executing blocks from multiple different networks
- Shahak clarified that the original plan was for Kademlia to separate nodes, but this doesn't work when directly dialing without using Kademlia for discovery

### 5. Malachite-Consensus Integration Progress

[Adi Seredinschi](https://github.com/adizere) reported significant progress:
- The first iteration of interoperability tests is now considered complete
- Disconnection issues have been resolved, with blocks being produced regularly
- Next steps include more in-depth testing:
  - Testing with latency
  - Testing with larger networks (beyond the small few-node networks used so far)
  - Producing non-empty blocks (current tests use empty blocks)
- The team is coordinating with Matan and Shahak on these fronts
- Both ADRs (Architecture Decision Records) are either merged or close to being merged
- The ADRs were sparked by conversations with the Madara team about three weeks ago
- Last week they had a review session with Trantorian that provided additional context
- Adi noted that Pathfinder is expected to start integration after v0.13.5, possibly this week or next
- A new workstream has started with Matan, reviewing the three sub-protocols that ensure liveness
- These protocols are being carefully scrutinized, though there's no specific timeline for completion

### 6. L1 Data Extraction Tool Update

[Leonardo Lerer](https://github.com/leo-starkware) acknowledged limited progress on this initiative:
- The team has had other priorities
- While they believe this tool should exist, it's not currently highly prioritized
- Mentioned that Vav from Pathfinder is working on it, and they hope that work will be sufficient

[Vav](https://github.com/vvav) provided additional details:
- The project is in the feasibility stage
- He has developed a tool that downloads and parses blobs
- Different blob versions are being addressed, with the first version working
- The second version, which adds stateless compression, is working but not yet integrated with parsing
- Integration is expected within the week

### 7. AOB (Any Other Business)

No additional topics were raised.

The meeting concluded with Aayush thanking everyone for their participation and reminding them that the next call would be on April 10th at the same time.

-----
### Attendees

- Aayush Giri (Moderator)
- Rodrigo Pino
- Vaclav | EQ
- Krisztian | Equilibrium
- Leonardo Lerer
- Wojciech Zieba
- Fireflies.ai Notetaker
- Trantorian (Kasarlabs)
- Chris
- rian
- Carmen
- Ariel Elperin
- Adi Seredinschi @Informal
- Eitan Moed
- Nikša (EQ)
- Vladislav Lazarev
- JB | Kasar
- Shahak Shama | Starkware

------------
Note: These Starknet All Core Devs Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **April 10, 2025**, at **11:00 AM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.