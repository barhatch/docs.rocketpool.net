---
next: 
  text: The Protocol DAO 
  link: "/guides/houston/pdao"
---

# The Houston Upgrade

The Houston upgrade is largely aimed at introducing a fully on-chain DAO to govern the protocol, known as the Protocol DAO or pDAO. It is a DAO like no other and it does not require snapshot voting or any other 3rd party tools to function, it is truly on-chain and one of a kind, more on that below.

This upgrade will also be introducing some other very exciting features that will allow new integrations and platforms to be built on the protocol. Some of these include the ability to stake ETH on behalf of node (not just from the node itself) and a new RPL withdrawal address feature that can allow the node operator to supply the ETH for staking and another party to trustlessly provide the RPL for the insurance bond.

## Protocol DAO

The Rocket Pool Protocol DAO (pDAO) is responsible for shaping the direction of the protocol and is run by RPL governance. Its members and their voting power are made up of node operators, big and small, all of which are directly participating in the protocol.

Typically DAO governance in the wider crypto space is done by token weight voting. Basically, the more tokens you hold for a protocol/project, the bigger your voting power. You also don’t need to be actively participating in the protocol, simply holding the tokens is enough.

This style of governance we wanted to avoid. If you want to help direct and guide the future of Rocket Pool, you need to be actively involved, not just storing tokens in a cold wallet. From the biggest Venture Capitalist funds to your ordinary guy running a single minipool, you will need to be actively participating in the protocol to help govern it.

Currently the protocol DAO has control over a variety of on-chain settings that are used in the protocol. New Rocket Pool Improvement Proposals (RPIP) can be made and voted on by these Node Operators within Rocket Pool. You can see the [**current RPIP registry here**](https://rpips.rocketpool.net/). If you’re a devil for the details, the current RPIP for the on-chain protocol DAO talked about now can be found here.

### What can the pDAO do?

The pDAO has control over many settings of the protocol, it can spend treasury funds and in our Houston upgrade, it comes with a new security council to help react quickly in the event of any potential issues with the protocol. Lets talk about each of those a little more below.

**Protocol Parameters:** These control certain facets of the protocol such as the setting that controls the minimum ETH amount that can be deposited for rETH (currently 0.01 ETH) or even controlling the maximum size of the deposit pool, this is how much max ETH can be deposited into the protocol before being assigned to node operators for staking. You can find a full table of [these settings here](https://rpips.rocketpool.net/RPIPs/RPIP-33#parameter-table).

**Treasury Funds:** RPL has a 5% inflation rate and a portion of that is allocated to the pDAO treasury. The pDAO have the ability to spend this treasury on a variety of protocol oriented endeavors, from funding development of the protocol directly, grants management for funding 3rd party improvements and projects that make use of Rocket Pool and more. Our Houston upgrade adds a new ability where these payments from the treasury can be done not just in a lump sum manner, but in a progressive manner to help track goals in relation to ongoing funding.

**Security Council:** As the Houston upgrade moves the pDAO to a fully on-chain system, a new safety measure was introduced in the form of the [security council](https://rpips.rocketpool.net/RPIPs/RPIP-33#security-council). These members can be elected by the pDAO and they have the ability to propose and execute changes quickly to pause the protocol in the event any potential issues occur. Quorum is met among members must be met for any security response proposals to be executed. The pDAO also has the power to remove members or disband the security council entirely if they need to.

### Proposals and Voting

For any governance system to function, there needs to be proposals and voting. At the moment, snapshot voting is used for these settings and proposal changes, then some manual intervention is needed to execute the changes. With the introduction of the [Houston upgrade and RPIP-33](https://rpips.rocketpool.net/RPIPs/RPIP-33), this is moved to a new optimistic fraud-proof system that allows any node operator to raise, vote on or challenge proposals, directly on-chain without the need for any 3rd party tools.

**Proposing:** Any node with a non-zero voting power may raise a proposal at any time. When doing so they must lock a proposal bond in the form of RPL for the entire proposal process.

**Challenging:** If a node that created a proposal has been found to have done so with incorrect data required, they can be challenged and the challenger must provide a bond for the challenge. The node that makes the challenge can be rewarded with the proposers bond made when creating the proposal if successful, however if they have made an invalid challenge, the proposer can collect their challenge bond.

**Voting**: If a proposal passes the period where it can be challenged, it enters the voting period. Node operators can then choose to vote in one of the follow flavors:

1. Abstain: The voter’s voting power is contributed to quorum but is neither for nor against the proposal.
2. For: The voter votes in favor of the proposal being executed.
3. Against: The voter votes against the proposal being executed.
4. Veto: The voter votes against the proposal as well as indicating they deem the proposal as spam or malicious. If the veto quorum is met, the proposal is immediately defeated and the proposer loses their bond. This is to dissuade spam, low quality proposals, or proposals that have not gone through off-chain processes first such as signaling by snapshot voting.
Once both voting periods have passed and the proposal is successful, the proposal can be executed and the change is applied to the Rocket Pool protocol.

After the proposal has passed the voting periods, the proposer may unlock their RPL bond, unless the proposal was defeated by a challenge or vetoed.

## Stake ETH on Behalf of a node

[RPIP-32](https://rpips.rocketpool.net/RPIPs/RPIP-32) allows an account to [stake ETH on behalf](../houston/stake-eth-on-behalf) of a Rocket Pool node that is registered in the protocol. This supports a variety of situations where the node operator is not directly providing the ETH:

- Enhanced security for node operators, as they can stake directly from their hardware wallet, eliminating the need to transfer funds to the node beforehand.
- Staking as a Service providers where custody of funds are managed by a trusted custodian.
- Protocol integrations where custody of funds are managed by smart contracts.
- DAOs or organizations where custody of funds are managed by a treasury.

While the primary aim of this feature is to facilitate single depositor scenarios, it’s worth noting that multiple independent depositors can also leverage this capability by creating smart contracts layered on top. Rocket Pool also introduced the ability to stake RPL on behalf of back in our previous Atlas release.

## RPL Withdrawal Address

Rocket Pool currently allows node operators to specify a withdrawal address for their ETH and RPL. This could be an external hardware wallet or something similarly secure.

With [RPIP-31](https://rpips.rocketpool.net/RPIPs/RPIP-31), you can set a withdrawal address for your ETH and [a new one for your RPL](../houston/rpl-withdrawal-address) if you wish. The RPL withdrawal address if set will be able to trigger and claim RPL from inflation rewards and will have no effect on ETH consensus rewards or anything related to ETH.

This creates some interesting opportunities where RPL can be trustlessly supplied by an entity to a node operator that does not wish to have exposure to RPL. That entity can then claim RPL rewards for putting up the required insurance collateral for the node.


## Time-based Balance and RPL Price Submissions 

Rocket Pool nodes need to have at least 10% collateral in RPL staked to be eligible for rewards, with their "effective stake" calculated based on the ETH:RPL ratio, which is reported by the Oracle DAO at the end of each rewards interval. Previously, this "top up window" (the time between the final RPL report and the end of the interval) had some uncertainty and fluctuated from interval to interval because it was being specified by number of blocks. This was valid pre-merge but didn't factor variability and randomness in the way blocks are added. 

To address this, the intervals for price and balance reporting will now be based on seconds rather than blocks. This change ensures predictability and has parity with the way rewards intervals are calculated today. If the interval is set to `86400` seconds (number of seconds in 24 hours), prices and balances are reported at the same time every day. 

The protocol now has a fixed and controllable "top up window," removing guesswork and providing users with a consistent 24-hour window for topping up after the final price update. Feel free to read more about this change in [RPIP-35](https://rpips.rocketpool.net/RPIPs/RPIP-35). 