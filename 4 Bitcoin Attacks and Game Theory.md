# Pool Strategies

## Pool Hopping

* Proportional payout decreases per share as more shares get submitted, marginal rewards lower
* Pay-per-share payout is always constant per share, marginal rewards always the same
* Miners can mine under proportional payout and when switch to pay-per-share later
* Proportional schemes incentivizes miners to submit nonces but not loyalty, not viable

## Pool Cannibalization

* In pay-per-share, miners are not incentivized to give their mining pool valid blocks
* Get paid for work without submitting valid blocks: pools "consume" other pools
* Distribute power into other pay-per-share schemes without submitting valid blocks
* Increase personal product to the detriment of other pools, very hard to detect in small amounts
* Distribute mining power over other pools but withhold valid blocks

## Nash Equilibrium

* Analyze the competition for resources using economic game theory
* Two theoretical pools in the Bitcoin network: Pool 1 and Pool 2
* Pools want to attack each other, but if both attack, both will be worse off
* Both pools attacking each other is a Nash Equilibrium
* Even if Pool 2 stops attacking, Pool 1 will not stop attacking to increase profit
* Rational actors will eventually choose the Nash Equilibrium, continually attack
* Pools can attack each other anonymously, large incentive to attack

# Double Spending

## Race Attacks

* Race Attack: timing of transactions affects the outcome
* Alice sends Bob 100 BTC, and Bob sends Alice a good immediately after
* Alice sends Bob a valid transaction and the rest of the network a conflicting transaction
* The conflicting transaction sends the 100 BTC to a different address that Alice owns
* Alice makes the transaction fee for the conflicting transaction higher, and it gets added to blockchain

## Confirmations

### Confirmation Basics

* Bob gets to keep the BTC if the transaction makes it into the longest chain, which can be forked
* Bob's transaction gets a number of confirmations: number of blocks built off a block
* More transactions means harder to successfully double spend
* Malicious miners have to fork before transaction and mine faster than the honest chain to surpass it

### Confirmation Demo

* Bob waits for k=2 confirmations to send the good: assumes the transaction is finalized after that
* Alice mines on a private chain that is not revealed to the rest of the Bitcoin network
* Alice broadcasts the chain, which the network accepts, and the honest chain is invalidated
* Depends on the ability to produce a chain longer than the honest network

## Disadvantages

* If double spending is discovered, the Bitcoin network would plummet
* Specialized hardware would also be useless, hardware can only mine BItcoin
* "Goldfinger attack": buy 51% of hash power and tank the network, destroying assets
* Can also short the Bitcoin currency with advantage of this devaluation

# Censorship Attacks

## Naive Censorship

* Censorship: ignoring transactions from an individual or group, isolating them from the network
* Blacklisting: Alice instructs all of her mining pools to not include Bob's transactions
* Only effective when Alice owns 100% of hash power in the network
* Doesn't fully block Bob from the blockchain, only causes delays and inconveniences
* Eventually, by sheer luck, miners will include Bob's transactions in a block

## Malicious Forking

### Punitive Forking

* Alice's mining pools have >50% of network hash power
* Can make her pools refuse to include any of Bob's transactions and announce this to everybody
* If miners include one of Bob's transactions, Alice will fork the chain and create a longer one
* Blocks containing Bob's transactions are rendered invalid
* Non-Alice miners will stop including Bob because blocks with Bob will never reap mining rewards

### Feather Forking

* Works even if Alice has <50% of network hash power
* Alice will attempt to fork if she sees a block with Bob's transactions but will give up after k confirmations
* Alice has a relatively small chance to censor Bob's transactions
* Miners also have a chance that their block will be orphaned: have to decide whether to include Bob
* Therefore, Bob must ramp up his transaction fees to comparatively large amounts

# Selfish Mining

* Miner keeps a discovered block secret and works on the next working block, the only one to do so
* Also used in the context of mining pools: submit shares but withhold blocks
* If your secret chain can outrace the honest one, you have the longest chain
* Can publish your secret longest chain at any point, reaping therewards
* Assumes that you can outrace the honest chain
* If the network catches up to you, it becomes a race to propagate the new blocks
* Malicious strategy is be profitable even with >25% of hashpower, if you can tell 50% of network about your block

# Defenses

## Block Validation

### Validation Basics

* Validate blocks using dummy blocks that contain signatures for that block
* Look at block's corresponding dummy blocks to verify how many users have seen/signed off on the block
* Selfishly-mined blocks would not have sufficient signatures
* When malicious miner publishes longest chain, all but the first block are rejected for lack of sigs

### Validation Vulnerabilities

* Vulnerable to Sybil attacks: selfish miner can generate fake signatures to validate blocks
* No way to determine how many signatures is enough to certify block validity
* Implementation would require an undesirable hard fork

## Fork Punishment

### Punishment Basics

* Punishes anyone who forks the blockchain
* Competing blocks receive no block reward, whether mined selfishly or honestly
* Miner who proves that there was a fork gets half the forfeited rewards
* Disincentivizes malicious users to fork, and incentivizes honest users to report forks

### Punishment Drawbacks

* Honest miners suffer collateral damage from fork punishment, constitutes a different type of attack
* Must change reward distribution and coinbase transactions of fork blocks
* Also would require an undesirable hard fork

## Tie-Breaking

### Uniform Tie-Breaking

* Consider each submitted block at each block height: uniform tie-breaking
* Protects against selfish miners who have a network advantage
* Miners don't take the first seen block as valid: wait to hear about competing blocks
* If there are multiple blocks at the same block height, miners randomly choose which to mine on

### Timestamps

* Timestamps issued by trusted party and incorporated into every block, publicly accessible and global
* Blocks are in competition if they are received in roughly the same timeframe
* Miners will always pick the block whose timestamp is fresher/more recent

### Tie-Breaking Drawbacks

* Defenses are easily bypassed if the malicious miners have enough hash power
* Requires a trusted third party, Bitcoin aims to be decentralized as possible

## Publish or Perish

### Publish or Perish Basics

* Backwards-compatible due to no hard fork, and disincentivizes selfish mining even with longest chain'
* Secrete blocks do not help selfish miners at all: amending Bitcoin's Fork Resolving Policy
* Bitcoin currently have a length-dependent FRP, this moves to a weighted FRP
* Weight a measure of exposure that a block gets in the network

### Weight Definitions

* τ is the assumed upper bound on the time it takes to propagate blocks across network
* "in time" for a valid block means:
  * height value is greater than that of local head
  * height value is same of local head but was propagated within τ time
* Uncle: "in time" with parent and one less the height of a block
* Weight = # of in-time blocks + # of in-time uncle blocks

### Weight Mechanics

* Verify weight through embedded hashes of blocks that count towards a block's weight
* Calculate weight from chain starting from block after root
* Withholding blocks won't be in time and will have less weight

### Weighted FRP

* If one chain is longer height-wise than the others by k or greater blocks, miners will mine on it
* Otherwise, miners will choose chain with largest weight
* If there is a tie for largest weight, miner chooses one randomly

### Selfish Disincentivization

* Block propagation race: Miner has a secret block, and a competing honest block is published
* Miner can choose to publish or to keep the block secret
* Neither option gives weight to selfish chain while not giving weight to honest chain

### Limitations

* Assumes synchrony, cannot assume if all blocks at a certain block height have been delivered
* Inconsistent views among honest miners if attacker broadcasts right before it becomes alte
* Not all miners could update software to publish or perish at same time
* Attacks could launch attacks on users while this desynchrony exists
* Network latency creates natural forks that would not be allowed under this sytem
* Doesn't consider collusion between multiple selfish miners and transaction fees

# The Bitcoin Network

## Peer-to-Peer

* No central entity sending or receiving message for up
* Messages get sent through flooding: you tell your neighbors who tell their neighbors ad infinitum
* Getting connected and joining network happens through recursion
* Initially get connected to seed peers, ask them for neighbors, pick some ad infinitum
* Continues until you reach 125 connection: 8 outbound, 117 inbound

## Network Topology

* How the network looks if you make it into a graph
* Want an even topology: nodes have the same weight, are uniform, and have random connections
* Some nodes have a lot greater mining power than others
* Mining pools don't have to be directly connected to Bitcoin network: makes nodes more powerful
* Uneven distribution of hash power leads to topology being hard to distinguish

## Network Latency

* Different propagation times lead to inconsistencies between miners
* If two miners find a valid block at the same time, the better-connected one propagates first
* Leads to disproportionate profits

## Network Attack

* Can leverage network topology and latency to launch a Sybil attack
* Can flood network with lots of nodes, can Sybil attack honest miners through network dominance
* When nodes hear about an honest block, nodes can ignore it and flood network with selfish block
* Well-connected and low-latency nodes that are well-connected can propagate secret blocks
* Still many other attacks and defenses that take into account network latency and topology