# Basic Concepts

## What is Bitcoin?

### What is Bitcoin?

* First and most widely used crpytocurrency
* "Bitcoin" is the protocol governing the currency, "bitcoin" are the actual units of currency
* Inspiration for the blockchain, underlying data structure of Bitcoin
* Blockchain stores every transaction that ever occurred in the history of Bitcoin

### Bitcoin Network

* Bitcoin network validates transactions and stores history
* Group of users communicating with each other as part of Bitcoin protocol
* Serves as a decentralized substitute for a central bank

### Problems with Decentralization

* No central parties to verify info about user accounts
* No protocol to kick out malicious users
* Double spending attack: some value used for more than it's worth
* Solved through blockchain and proof-of-work consensus protocol

## Bitcoin vs Banks

### Purposes of Banks

* Manage and verify accounts to prevent stealing
* Must provide identification before transactions can take place, tracking users
* Transfer and redeem money honestly on our behalf, keep track of account balances
* Provide trust: run by professionals

### Bitcoin Functionality

* Identity and account management autonomous
* Disconnected from real-world identity, provides privacy
* Peer-to-peer transactions: direct transactions are confirmed and validated through network
* Each user possessed individual copy of ledger, ensures integrity of data
* Decentralization prevents single points of failure, all nodes keep records
* Provide trust: Bitcoin protocol and publicly verifiable, tamper-proof ledgers are correct

# Bitcoin from the Ground Up

## Identity

### Role of Identity

* Authentication with integrity, nobody can act on our behalf
* Authentication: only you have access to your account
* Enforce blaming: can call out malicious activity with proof
* Integrity: no authentication methods can be replicated by anybody else

### Public and Private Keys

* Verity the integrity of Bitcoin identities using public keys and private keys
* Public identities are what you give out to public for communication and identification
* Private keys are secrets that only you should own, give access and idenity
* Public keys used for receiving, private keys used for redeeming
* Received money associated with public key, and you spend money with private keys
* In Bitcoin, users send transactions to your address, derived from public keys
* Necessary because there are no authorities to generate identities for users
* Generation of identity: private key at random, public key generated through private key
* Ensures two users do not end up with the same identity, no overlaps

## Transactions

### Validity

* Valid transactions have proof of ownership, available funds, no other transactions use same funds
* Proof of ownership is a signature, bank verifies your funds and that you don't spend same money twice
* Bitcoin enforces this with a UTXO/Unspent Transaction Output model

### UTXO Model

* In banks, all our funds are aggregated into one acocunt
* Decentralized networks cannot keep track of accounts
* Users spend directly from transactions made from them, like piggy banks
* When transactions are made to us, we put money into a UTXO
* When we want to spend money, we dissolve entire UTXO and place leftovers into another UTXO
* Complexity of checking for transaction validity goes down. UTXOs can only be spent once
* Total amount of bitcoin you have is the sum of all your UTXOs

### Transaction Example

* Alice has two UTXOs with 100 and 50 Bitcoins each, wants to send 125 Bitcoins to Bob
* Alice inputs her first UTXO, and then her second UTXO (100 < 125)
* Transaction inputs are two UTXOs and one output UTXO worth 125 Bitcoin
* The remaining 25 Bitcoins go to a change UTXO with unspent Bitcoins to Alice

## Record Keeping

### Distributed Databases

* Need a database to store the history of transactions
* Need to store the history of transactions to confirm future validity
* Everyone keeps a copy of the ledger, as decentralized as possible
* Updating ledger constantly is very costly

###  Blockchain

* Every update to Bitcoin ledger is a "block" of transactions
* Blocks are chained to previous blocks, creating the "blockchain" data structure
* Do not have to update ledger after every transaction, database is discrete
* Good for identifying discrepancies between two different versions of the ledger
* Blocks contain info about the previous blocks
* Mutating blocks changes and invalidates future block info
* Reduce strain of storing individual transactions and maintains consistency

## Consensus

### Agreement

* Necessary to make sure everyone agrees on history of transactions
* Users must agree on valid updates to make sure no corrupted info is accepted
* Distributed databases need agreement to function
* Need some mechanism so everyone can come to consensus

### Double Spend Attack

* Promising the same money to multiple transactions at once
* The recipients only know about their incoming bitcoins and not any other transactions
* Problem when trying to redeem the same bitcoins from two different people
* Impossible to come to consensus when entities only see transactions that involve them

###  Peer Validation

* System of proposers and voters instead of siloed decision
* Person who wants to make a transaction makes proposal about update
* All others on network vote based on whether transaction is valid or not
* Transaction saved with a majority of votes
* Protects against double-spend attack, peers in network just vote no

### Multiple Identities

* Identity theft: can create addresses and identities easily
* Make multiple identities to cast more votes than allowed for malicious transaction
* Sybil attack: users create multiple identities and votes become meaningless
* Therefore, we cannot assume that each online identity deserves the same voting power
* Bitcoin must make votes expensive despite the number of identities one must have

### Votes with Resources

* Therefore, votes must not be cast with identities, but must be case with resources
* Computing power as resources, making voting power scarce
* "1-CPU-1-Vote" system called the "Nakamoto Consensus"

###  Proof-of-Work

* Method by which users provide evidence of spending resources
* Translates computer power to voting power, and method by which users vote
* When someone wants to make a proposal, they must solve a computationally difficult problem
* Problem uniquely generated based on info within proposed block
* Computer basically brute-forces solution, and then submits input and proposed block
* Eliminates Sybil attacks by tying voting power to real-world identities anonymously

### 51% Attacks

* Bitcoin assumes that the majority of computing power is honest
* If a majority of the computing power is malicious, it can create accepted alternative chains
* Allowing an honest minorities to control transaction history makes it centralized

### Forking

* Different miners can create different blocks to add at the same point
* Creates multiple different versions of transaction history
* Blocks are competing at same block height, and there has been a fork
* Some purposeful forks make changes to Bitcoin protocol