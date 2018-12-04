# Wallets

## Wallet Basics

### Purpose of a Wallet

* Secure and keep track of identity/public key and protect against identity theft with wallets
* Wallets are just methods of storing and accessing private keys, allowing you to spend bitcoin
* Software will also store, send, receive, and list your transactions

### Types of Wallets

* Hot wallets are connected to the internet, cold storage is not
* Hot wallets: smartphone apps and online web wallets
* Cold storage: paper, hardware, and just remembering your private keys
* Some paper wallet services on websites can be extremely secure
* Hardware wallets are trusted execution environments separate from your computer
* Can remember a set of words and then hash them together to get your private key
* Key stretching: hash your brain wallet a large number of times, very hard to brute-force

### Bitcoin Exchanges

* Trade between different types of traditional currency and cryptocurrencies
* Set the price of bitcoin and define market value
* Centralized exchanges: easy to use and access but poses a risk to funds
* Never safe to keep money in exchanges in long-term
* Decentralized exchanges: don't rely on third party to hold funds/do back-end work
* Trades occur directly between users (P2P), and can be trustless

## Wallet Mechanics

### Payment Verification

* Don't have to download the entire Bitcoin blockchain
* SPV: Simple Payment Verification, way to verify if your own transactions are included in a block
* Only have to download block headers instead of entire block with transactions
* SPV just runs a Merkle proof of inclusion using the headers
* Clients that run SPV are lightweight/thin clients

### Key Generation Best Practices

* Never reuse pseudonyms: generate new public/private key pair for every transaction you make
* Makes your bitcoin less traceable, difficult to determine what you've been spending bitcoin on
* Compromising one key will not necessarily mean your entire bitcoin supply is lost
* Generation of public and private keys is not computationally intensive anyway
* Wallet software usually follows these best practices

## HD Wallets

* Help you manage an increasing number of public/private keys
* HD Wallets: Hierarchical Deterministic Wallets
* Start off with random seed, and then deterministically generate new key pair from that seed
* Can take the hash of a master key, append it to some counter, and then hash
* Lessens the load on wallets having to keep track of all your keys
* Also useful for hierarchical structures, such as companies, with president holding the master key

# Mining

## Mining Steps

### Overview

* Bitcoin miners must download the entire Bitcoin blockchain, to verify future transactions
* Verify incoming transactions: fill up our block with valid transactions
* Create block using given transactions and metadata
* Find proof-of-work: valid nonce that solves partial preimage hash puzzle
* Broadcast your block if no competitor blocks exist
* If block gets included in longest chain, you get a bitcoin reward

### Verify Transactions

* Miners must verify Bitcoin-based transactions
* Store unverified transactions in the "mempool": memory supply
* Choose transactions to verify based on transaction fees, run unlocking script
* If the code successfully unlocks the bitcoins, the transaction goes through

### Block Generation

* Gather transactions into a block: choose which ones you want
* Need several pieces of info to construct a block, such as a Merkle Root and other metadata

### Finding a Nonce

* Miners need to find nonce which makes the hash of block header less than some value
* Translate energy burned into computation directly to voting power through Proof-of-Work
* Nonce in header is a small number (32-bit), must change coinbase nonce and Merkle Root also
* This yields an entirely different hash puzzle to solve

### Broadcasting

* Miner broadcasts the nonce ASAP to other miners, which will validate the new block
* Then, the miners abandon their previous blocks and start working on this new longest chain
* Proof-of-Work is a random lottery: cannot guarantee that someone else has not broadcasted block

### Profit

* If block makes it into longest chain, miner receives block reward and transaction fees
* If our block is competing with another block submitted at the same time, it's random
* Orphaned/forked blocks which are not in the longest chain do not profit

## Mining Profit

### Components of Profit

* Mining Revenue = Block Reward + Transaction Fees
* Mining Cost = Fixed Costs + Variable Costs
* $$$ Profit $$$ = Revenue - Cost
* Most mining operations don't turn a profit for a long time

### Block Reward

* Most significant source of profit for miners
* Reward that goes to miners whose block is included in the longest chain
* Miners include the coinbase transaction, a special transaction to themselves
* Allows for the minting of bitcoin and incentivizes honest actors to validate blocks

### Bitcoin Supply Cap

* Supply cap tapers off at 21 million bitcoins
* Block reward halves every 210,000 blocks, or around every 4 years
* Lost private keys and burnt bitcoins also decrease supply cap

### Transaction Fees

* Price set by sender of transaction, cost of service for using the Bitcoin network
* Not required for transaction to go through, but incentivize miners to use your transaction
* Miners can fit only so many transactions in their blocks: blocks have a size limit of 1 MB
* As block rewards go down, transaction fees will become the primary source of profit for miners

### Fixed Costs

* Hardware: allows users to mine, one-time purchase
* Mining power has grown exponentially since the start of Bitcoin
* CPU -> GPU -> FPGA -> ASIC

### Variable Costs

* Energy consumed in mining: embodied energy, electricity, and cooling
* Infrastructure and overhead: space and employees, may need to get warehouses and maintainers

## Mining Pools

### Mining Pool Basics

* Individual miners join their hash power together, collectively find blocks
* Run by pool manager/pool operator, which distributes jobs to miners

### Insurance

* Shares: near-valid or valid blocks, used to prove that miners are spending computation power
* Ensures that no miner in the pool can take the rewards for themselves
* Miners solving problem based on Merkle Root, which is based on the coinbase transaction
* Coinbase transaction goes to mining pool manager
* Changing the address of the transaction prevents calculation of shares

### Distribution of Pay

* Pay-per-Share and Proportional payout schemes
* Pay-per-Share pay a fixed amount of money per share, guaranteed and constant per share
* More advantageous for miner, as variance of payout reduced, but pools take on more risk
* Also no incentive for miner to actually submit valid blocks to pool
* Proportional pay when valid blocks found, reward proportional to shares submitted
* Proportional schemes are rare due to risks incurred by the miners
* There exist many other ways to pay miners, depends on size and assumptions of pool

### Pros

* Give individual and smaller miners the opportunity to make profit more easily
* Software changes easy to make, only one person running full node for the pool

### Cons

* Must trust the pool manager to actually pay, consequence of centralization
* Various attacks are enabled by mining pools
* Large pools are disliked, because of dangers of approaching 51% of network
* Single entities can hide total amount of mining power, "laundering hashes"
* Leverage mining power without revealing it to the community