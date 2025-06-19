# Starknet All Core Devs Meeting #28
## Meeting Details

- **Date & Time:** Thursday, June 5, 2025, 11:00-11:30 AM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/live/EEqyonZXIm4
- **Agenda:** https://github.com/starknet-io/pm/issues/18
- **Moderator:** Leonardo Lerer

## Meeting Notes

The meeting began with Leonardo Lerer welcoming everyone and noting he would be substituting for Aayush as moderator for this call, outlining updates on v0.14.0, Staking v2, and client team progress.

### 1. Starknet v0.14.0 Testnet Deployment

Leonardo provided comprehensive updates on v0.14.0 deployment progress. The version is still currently on integration where they are running stress tests. He announced specific estimated upgrade dates: testnet deployment is planned for June 23rd and mainnet deployment is scheduled for July 28th. Leonardo noted that most participants should be aware of these dates since he had posted them in several Slack channels.

The main item relevant to this forum is RPC 0.9, which currently exists only as a pull request with no release candidate (RC0) available yet. Leonardo expressed hope to publish the RC as soon as possible, potentially today or very early next week. The reason for the delay is that they're trying to internally converge on an API for the feeder, which has consequences for the RPC as well. He emphasized the need to converge quickly on this and planned to share an RC announcement soon.

Leonardo reported that external partners are actively switching to RPC 0.8 and sending V3 transactions, describing this as a significant effort across the ecosystem that appears to be on track. He reminded everyone that all other transaction versions will be deprecated in the upcoming v0.14.0 release, with only transaction V3 sent with RPC 0.8 being accepted, and all others being rejected at the gateway.

### 2. Staking v2 Implementation Status

Natan Granit provided detailed updates on Staking v2 deployment, apologizing for his previous absences from calls. He confirmed that the launch date remains June 17th (Tuesday), which is less than two weeks from the meeting date. The attestation contract has already been deployed, and Natan shared the address in both the joint channel and telegram channels.

Natan outlined the expected validator workflow and schedule. Validators should start running the attestation package prior to the upgrade to ensure they're ready for the first epoch, rather than coordinating 100 validators to start at the exact same moment. He recommended starting either Tuesday morning or Monday night before the upgrade. Currently, if a validator runs the attestation package before the contract upgrade, they would experience repeated errors every 5 seconds until the contracts are upgraded.

The client teams confirmed their behavior during this pre-upgrade period. For Pathfinder, Krisztian explained it would result in read failures when trying to fetch relevant details from the staking contract for particular stakers. Rodrigo confirmed the same behavior for Juno. Natan was satisfied that these would be read failures rather than transaction reversions, avoiding network congestion issues.

Krisztian mentioned that Pathfinder updated their version with a quality of life feature - building in the attestation address so users don't have to manually configure the contract address. This was purely a convenience improvement with no logic changes, so older versions would work equally fine.

Regarding configuration differences, Natan noted that contract parameters on testnet and mainnet are not exactly the same but not order of magnitude different. The main change is epoch length - from 20 minutes on testnet to around an hour on mainnet. The teams confirmed this shouldn't affect underlying logic since they query the contract rather than using hard-coded configuration values. Natan also mentioned they would run the packages themselves to test and monitor liveness during the first epoch, providing a backup set of validators to help distinguish between package issues and network issues.

### 3. Cairo Native Deployment Strategy

This agenda item was not specifically addressed in the meeting.

### 4. Client Team v0.14.0 Integration Status

**Juno** (Rodrigo Pino):
- No major issues currently, continuing development of Tendermint and new database
- Plan for Tendermint to be in testable state around end of June for July testing, consistent with previous timeline
- For Starknet v0.14.0: Were using a specific tag from blockifier since release candidate wasn't available during their implementation, now planning to update with the release candidate, deploy it, ensure everything works, and make a pre-release with v0.14.0 support

**Kasar Labs** (Trantorian/JB):
- Continued progress on v0.8.1 integration of missing WebSocket methods, progressing nicely and should be completed sometime next week
- Currently merging latest Cairo Native features into Madara and working on blockifier v0.15 PR
- Moving forward tightly with blockifier RPC development

**Pathfinder** (Krisztian):
- Released next beta version with support for Starknet v0.14.0, including new blockifier and compiler upgrade
- Still missing JSON RPC 0.9 support but started planning for it - will add support for new transaction statuses as soon as feeder gateway API and JSON RPC spec are finalized, then release next pre-release version
- Working on fine-tuning for pruned mode operation where only configurable part of history is kept in database rather than full history
- Consensus work continuing, though Abel is on vacation this week, still working on integrating Malachite and proposal validation

### 5. Consensus Implementation Progress

Adi Seredinschi provided comprehensive updates on Malachite consensus implementation. The team considers the interoperability tests finished, having successfully debugged the slow block production issue and added support for non-empty blocks in the interoperability tests. They fixed all obvious bugs found during this testing process.

They considered two additional tests out of scope: rotating validator set and equivocation testing, since these don't seem relevant for the very foreseeable future in production. They can revisit these in a month or two when they may become more relevant, but are trying to maintain focus on immediate priorities.

The major update concerns the protocols and protocol files for guaranteeing liveness of block production. There were multiple back-and-forth reviews and conversations around the protocol, and Matan was convinced the current version is acceptable. However, the protocols are not yet finalized, so they won't proceed with testing until finalization occurs. Once protocols are finalized, they'll conduct one more sanity check to ensure no bugs were introduced in liveness protocols, which will complete the interoperability testing and allow them to merge the PR on the Malachite side.

Regarding the testing regimen for Malachite, Adi explained they're proceeding internally with two different approaches to frameworks and infrastructure for large-scale testing. One approach has a steeper learning curve and uncertain long-term viability but appears more flexible - it's based on Kurtosis infrastructure management. The other approach is simpler with existing team expertise - using Terraform with Docker and deploying on Digital Ocean. They're exploring whether the Terraform approach can be upgraded or made more flexible with Kurtosis, currently testing both options with Malachite-based sequencer and additional applications.

When asked about the root cause of the slow block production issue they debugged, Adi explained it was a rather naive problem involving a sleep function in the Starkware sequencer. Through detailed log analysis, they found the issue occurred when the Starkware sequencer wasn't proposing - it was sleeping, potentially to simulate execution, signature verification, or other processes.

Once the Juno sequencer is ready and liveness protocols are finalized with authorization for large-scale testing, they'll be prepared to reuse their internally built tooling for broader testing initiatives.

### 6. AOB (Any Other Business)

Leonardo opened the floor for any additional questions, issues, or topics. No additional business was raised by participants.

-----
### Attendees

- Leonardo Lerer (Moderator)
- Rodrigo | Nethermind
- Natan Granit
- Krisztian | Equilibrium
- Wojciech Zieba
- Malicki Maciym
- Shahak Shama
- JB | Kasar
- Vaclav | EQ
- Eitan Moed
- Adi Seredinschi @Informal
- Thiago | Nethermind
- Trantorian | Kasarlabs

------------
Note: These Starknet All Core Devs Calls occur bi-weekly at the same time. The next call is scheduled for Thursday, **June 19, 2025**, at **11:00 AM UTC**. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.
