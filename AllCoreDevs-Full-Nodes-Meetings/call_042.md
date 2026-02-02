# Starknet All Core Devs Meeting #42
## Meeting Details

- **Date & Time:** Thursday, December 18, 2025, 12:00-12:30 PM UTC
- **Duration:** 30 minutes
- **YouTube:** https://www.youtube.com/watch?v=Fn5Mcw2nz3E
- **Agenda:** https://github.com/starknet-io/pm/issues/32
- **Moderator:** [Aayush Giri](https://github.com/Giri-Aayush)

## Meeting Screenshot

![Call #42 Attendees](images/call_042_attendees.png)

## Executive Summary

This meeting marked the **final Starknet All Core Devs Call of 2025** and served as the mainnet v0.14.1 deployment retrospective. Key outcomes included:

- **Mainnet v0.14.1 deployment successful** - Deployed December 10, 2025 with approximately 10 minutes 40 seconds downtime (close to the 10-minute target)
- **Post-deployment stability confirmed** - All systems running smoothly; some users experienced 502 errors due to DNS caching (under investigation)
- **Contract migration from Poseidon to Blake completed smoothly** - No issues with block size during migration
- **2026 roadmap focus: decentralized validation** - Target is second half of 2026, with step-by-step approach prioritizing security
- **Published roadmap expected by next call** - Starkware finalizing 2026 priorities
- **All client teams stable post-deployment** - Pathfinder v0.21.3, Juno, and Madara/SNOS all performing well
- **No Malachite release until end of year** - Jasmina unable to attend; consensus updates deferred to January

## Meeting Notes

The meeting opened with [Aayush Giri](https://github.com/Giri-Aayush) welcoming participants to the final Starknet All Core Devs Call of 2025. He covered housekeeping items including the recording of the session and request to stay muted when not speaking. He noted this call was particularly special as the last call before the holidays and, more importantly, the mainnet v0.14.1 deployment retrospective.

Aayush outlined three main sections to cover: the deployment retrospective, strategic roadmap updates, and client team updates. He also noted that both Jasmina and Henry were unable to join, so consensus and poll updates would be covered in the January calls.

### 1. Mainnet v0.14.1 Deployment Retrospective

Aayush provided context that 8 days prior, on December 10, 2025, v0.14.1 was deployed to mainnet. In the previous call, confirmation was given that the deployment was on track with a dress rehearsal scheduled for December 8th targeting less than 10 minutes of downtime.

[Eitan Moed](https://github.com/eitanm-starkware) walked through the deployment experience. The deployment went relatively smoothly, with downtime lasting approximately 10 minutes and 40 seconds—very close to the expected 10-minute target. This calculation also included the time to send internal transactions to verify the network was operational before announcing the end of downtime.

**502 Error Investigation:**

Following the upgrade, some users experienced 502 errors. Eitan explained this is connected to the DNS system setup and DNS caching. The team expected this would last about 5 minutes, but it extended longer due to caching on the users' end. Different systems will eventually be in place to prevent such errors. The investigation into this issue continues.

**Contract Migration Success:**

The migration of all contracts from Poseidon to Blake hash function completed quickly and smoothly. This was one of the key challenges as the migration happened within blocks while also needing to include regular transactions. The team ensured blocks would not become too big to close during this process. Eitan confirmed there were no complaints about this aspect.

**Post-Deployment Stability:**

In terms of actual functionality, v0.14.1 has been running smoothly since the upgrade. The team is conducting proper investigations to understand any hiccups and address them in future deployments.

**Ecosystem Considerations:**

On the ecosystem side, one challenge was ensuring everyone had upgraded their technology stacks before the upgrade to avoid issues with declaring contracts. For the most part, almost everyone did upgrade. Occasional reports of complications with declaring are likely due to users not upgrading their stacks.

**Key Discussion Points:**
- Downtime of 10 minutes 40 seconds closely matched the 10-minute target
- 502 errors experienced by some users due to DNS caching issues
- Contract migration from Poseidon to Blake completed without blocking issues
- All v0.14.1 functionality running smoothly since deployment
- Some ecosystem participants still experiencing issues due to not upgrading their stacks

### 2. Strategic Roadmap Update for 2026

Eitan provided an update on the 2026 roadmap. Starkware has been discussing roadmap priorities internally throughout December, with a focus on reaching decentralized validation.

**Decentralized Validation Target:**

The current roadmap revolves around taking proper steps to reach decentralized validation in a secure manner. This means avoiding significant jumps from one upgrade to another, instead progressing slowly and surely. The target is to reach decentralized validation in the **second half of 2026**.

Eitan noted there is no specific date or month scheduled yet for when decentralized validation will be achieved. This will be determined based on initial milestones and the step-by-step process being put together.

**Staking Protocol Integration:**

One of the key questions being addressed is how to reach decentralized validation while bringing the staking protocol into the process. Eitan mentioned very interesting discussions are happening on this front.

**Published Roadmap Timeline:**

When Aayush asked if a published roadmap could be expected by the next call, Eitan confirmed he believes so.

**Key Discussion Points:**
- Decentralized validation is the primary focus for 2026
- Step-by-step approach prioritizing security over speed
- Target timeframe: second half of 2026
- Staking protocol integration is a key consideration
- Published roadmap expected by next call (mid-January 2026)

### 3. Client Team Post-Deployment Updates

Aayush transitioned to client team updates, asking each team to keep updates to 3-4 minutes.

#### **Pathfinder** ([Krisztian Kovacs](https://github.com/kkovaacs)):

Krisztian reported that Pathfinder v0.21.3 remains their latest release. They are not aware of any issues with this version either during or after the mainnet upgrade. The upgrade went fairly smoothly as far as Pathfinder is concerned.

The team is currently working on extending metrics and adding smaller improvements. There are no blocking or showstopper issues with Starknet v0.14.1 and the latest Pathfinder release. Krisztian expressed hope they will not need to cut another release before the holidays.

#### **Juno** ([Rodrigo Pino](https://github.com/rodrodros)):

Rodrigo confirmed Juno released in time for the mainnet deployment. They had a few small RPC issues where results were not fully consistent, which were fixed in the release. Additionally, a small issue with the Paradex network was resolved.

Since the beginning of last week, there have been no issues. The team is now focused on optimizing Juno, working on consensus implementation, and improving Juno's database and state management.

When asked about RPC v1.0 implementation feedback, Rodrigo clarified they are currently on RPC v0.10. He noted v1.0 will likely come when Starknet reaches decentralization, at which point all client teams will move to v1.0.

#### **Madara/SNOS/Karnot** ([Heemank Verma](https://github.com/heemankv)):

Heemank confirmed that for SNOS v0.14.1, they have released a pre-release which is available for review. For Madara, they have a branch that has been thoroughly tested and works perfectly well with v0.14.1. The public release is expected by the end of the week (before December 25, 2025).

**Orchestrator Progress:**

On the orchestrator side, the team has been doing significant optimizations to improve speed and performance. These have been stress-tested with many blocks on v0.14.0 and are performing well.

For orchestrator v0.14.1 support, they are currently blocked on the Herodotus team who are testing the aggregator. Heemank mentioned having a conversation with them that morning, and they indicated deployment would happen that day. Once complete, orchestrator testing can proceed.

**Cairo Native:**

Cairo Native is working well. All replays being done with Madara have been running with Cairo Native enabled without issues.

Heemank confirmed all releases (SNOS, Madara, and orchestrator with v0.14.1 support) are expected by the end of the week.

### 4. Consensus Update (Deferred)

Aayush noted that Jasmina from the consensus team was unable to join. She had sent a message indicating there will be no new Malachite release until the end of the year. A full consensus update will be provided in the January call.

### 5. Any Other Business

Aayush opened the floor for any other topics before the holiday break. With no additional topics raised, he proceeded to close the meeting.

### 6. Closing Remarks and Holiday Wishes

Aayush closed the call with holiday wishes, thanking everyone for joining the last call of 2025 and for all the updates shared. He offered several final notes:

**Congratulations:**
- Congratulations to everyone on the successful v0.14.1 deployment on December 10th. This was a significant milestone and the coordination across all teams was excellent.

**Next Call:**
- The next call will be in the new year with the specific date to be announced soon, likely mid-January 2026
- The call will return to the regular 12:00 PM UTC time slot

**Recording and Notes:**
- Recording will be published on YouTube
- Meeting notes will be shared on GitHub

Aayush wished everyone happy holidays, a wonderful break, and a great start to 2026, thanking all participants for their incredible work building Starknet throughout the year.

---

## Key Decisions Summary

1. **Mainnet v0.14.1 Deployment Confirmed Successful**
   - Deployed December 10, 2025 with ~10 minutes 40 seconds downtime
   - Close to the 10-minute target set in Call #41
   - Contract migration from Poseidon to Blake completed smoothly

2. **2026 Roadmap Focus: Decentralized Validation**
   - Target: Second half of 2026
   - Step-by-step approach prioritizing security
   - Staking protocol integration under active discussion

3. **Published Roadmap Expected by Next Call**
   - Starkware finalizing 2026 priorities
   - Public communication expected mid-January 2026

4. **All Client Teams Stable Post-Deployment**
   - Pathfinder v0.21.3 running without issues
   - Juno released and stable with minor RPC fixes applied
   - Madara/SNOS v0.14.1 releases expected by end of week

5. **No Malachite Release Until End of Year**
   - Consensus updates deferred to January call

---

## Action Items Tracker

| Owner | Action | Target Date/Call | Status |
|-------|--------|------------------|--------|
| Starkware | Continue investigating 502 DNS caching errors from deployment | Ongoing | In Progress |
| Starkware | Publish 2026 roadmap focused on decentralized validation | Call #43 (mid-January 2026) | Pending |
| [Eitan Moed](https://github.com/eitanm-starkware) | Finalize 2026 priorities and roadmap details | Mid-January 2026 | In Progress |
| [Krisztian Kovacs](https://github.com/kkovaacs) | Continue monitoring Pathfinder v0.21.3 post-deployment | Ongoing | In Progress |
| [Rodrigo Pino](https://github.com/rodrodros) | Continue Juno optimization, consensus, and DB improvements | Ongoing | In Progress |
| [Heemank Verma](https://github.com/heemankv) | Release Madara v0.14.1 public version | End of week (before Dec 25, 2025) | In Progress |
| [Heemank Verma](https://github.com/heemankv) | Release orchestrator v0.14.1 (pending Herodotus aggregator) | End of week (before Dec 25, 2025) | Pending |
| Herodotus team | Deploy aggregator for orchestrator testing | December 18, 2025 | In Progress |
| [Jasmina Malicevic](https://github.com/jmalicevic) | Provide full Malachite/consensus update | Call #43 (mid-January 2026) | Pending |

---

## Attendees

- **Moderator:** Aayush Giri | Nethermind
- Dat Duong
- egecaner
- Eitan Moed | Starkware
- Heemank Verma | Karnot
- Krisztian Kovacs | Equilibrium
- Maksym Malicki
- Rodrigo Pino | Nethermind
- @sistemd | Pathfinder
- Thiago Ribeiro | Nethermind
- Vaclav | Equilibrium

---

## Glossary

- **v0.14.1**: Current Starknet version deployed to mainnet on December 10, 2025; introduced Blake2 hash function
- **Poseidon**: Previous hash function used for contract hashing; migrated to Blake in v0.14.1
- **Blake / Blake2**: New hash function adopted in v0.14.1; all contracts migrated during deployment
- **502 Error**: HTTP error experienced by some users post-deployment due to DNS caching issues
- **DNS Caching**: Domain Name System caching that caused delayed propagation of updated endpoints
- **Decentralized Validation**: Moving from centralized sequencer to decentralized validator set; target H2 2026
- **Orchestrator**: Karnot component managing block production and proving; being optimized for performance
- **Aggregator**: Herodotus component needed for orchestrator v0.14.1 testing
- **Cairo Native**: Native compilation of Cairo code for improved performance; confirmed working well with Madara
- **SNOS (Starknet OS)**: Starknet operating system responsible for state transitions
- **Malachite**: BFT consensus engine implementation by Informal Systems; no release until end of year
- **RPC v0.10 / v1.0**: JSON-RPC API versions; v1.0 expected when decentralization is achieved
- **Pathfinder v0.21.3**: Latest Pathfinder release; stable for v0.14.1 mainnet
- **Paradex**: Layer 2 network running on Starknet; minor Juno issue resolved

---

**Next Meeting:** Mid-January 2026 (specific date to be announced), 12:00 PM UTC

**Note:** This was the final Starknet All Core Devs Call of 2025. The next call in January will cover 2026 roadmap details, consensus updates from the Malachite team, and continued client team progress. These calls occur bi-weekly at the same time. All interested parties are encouraged to join and contribute to the ongoing discussions and development efforts.

**Happy Holidays to all Starknet builders!**
