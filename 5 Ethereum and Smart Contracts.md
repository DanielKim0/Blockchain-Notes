# Overview

## Smart Contracts

- Code that facilitates, verifies, or enforces the negotiation or execution of a digital contract
- To each consensus, a trusted entity must run the code, must make sure it is enforced

## High-Level Overview

* Decentralized platform designed to run smart contracts
* Distributed computer spread among many nodes that runs code people feed to it
* Account-based instead of UTXO-based blockchain
* Nodes come to consensus of a "global system state", which transactions can change

# Features

## Accounts

* In Ethereum, private keys prove ownership of an account which tracks a balance
* Accounts more space-efficient, easy to look up and transfer accounts

## Types of Accounts

* Externally-owned accounts: owned by entity outside network, contain address and ether
* Can send transactions to transfer ether and trigger contract code
* Contract: owned by smart contract, contain address, code, and storage
* Code executed when other accounts make transactions to trigger function calls

## Smart Contract

* Autonomous agents inside Ethereum network
* Transactions call specific functions in contracts
* Direct control over internal ether balance, contract state, or storage
* Can store/maintain data, manage contracts between users, provide functions to other contracts, or authenticate

# Virtual Machine

## Compilation and Process

* Smart contracts are written in high-level languages (Solidity, Vyper, etc.)
* Must be compiled down to Ethereum Virtual Machine code (EVM code)
* Every node in the network executes the compiled code, and all in the same way
* Nodes come to consensus on new system state using proof-of-work
* Different block creation times (15 sec) and hashes (Ethash) than Bitcoin

## Proof-of-Work

* Randomly selected one node's execution result as correct based on hashpower
* All nodes run EVM as part of block verification, removes trusted third party
* Enables P2P agreements that are noted on the blockchain

## Gas and Fees

* If contract has infinite loop, attacks can launch DOS attacks on the entire network
* Gas fuels execution of a contract, all code requires gas to execute
* Transactions specify starting gas and fee per gas unit
* At start of transaction, startgas * gasprice ether are subtracted from sender
* Remaining gas is refunded to sender after execution
* End states: program ends or runs out of gas
* Gas is your fee for using distributed, trustless computational power
* Disincentivizes DOS attacks due to high costs

## EVM and Consensus

* Transactions go from old state to new state
* Start with current state, gas, memory, code, metadata, stack, and program counter
* Feed this into EVM, which outputs updated state

# Use Cases

## Smart Assets

* Smart asset/token on an existing blockchain: building your own currency on top of another
* User must have funds, be authenticated, and is not double spending
* Smart contract only needs to handle checking funds: storage and sending function
* Can tap into existing protocol and procedures (authentication and double spending)
* User only needs to provide basic, secure logic

## Multisig Wallets

* Use built-in authentication protocols to build multiparty authentication wallet
* m of n signature scheme, no one person has control over wallet

## Proof of Existence

* Put a hash of document on the blockchain: prove ownership of info without revealing it
* Prove that we were the ones who included hash on blockchain and immutability of info
* Document ownership: reveal the inputs late to prove ownership
* DNS System: Domain Name System, associated names and values on smart contract

## Land Titles

### Documents and Blockchain

* Problem: unclear documents, corruption,  no government authority
* Pitfalls: corruption, no government resources, lack of trust
* Track ownership of documents, association between users and document hashes
* Digital signatures to provide ownership, public info on blockchain
* Limits centralization and provides transparency and immutability

### Problem: GIGO

* GIGO: Garbage In, Garbage Out 
* Problem when linking real-world into to digital info
* Blockchain can handle internal data but cannot validate outside input
* Need to validate input before sending info to blockchain through "oracles"
* However, oracles are centralized, trusted third-parties

## Prediction Markets

* Allows users to bet on future and receive winnings
* With central organization, users depend on honest payouts, able to censor users
* Can use smart contracts to tap into blockchain protocol to handle market logit
* Execution of logic is public and anyone can join
* Incentivize certain behaviors: if user finds a bug, they can bet on bug bounties
* Futarchy: government controlled by prediction markets
* Citizens who predict outcome of policy will be rewarded, and the rest will lose
* Eventually, only the regularly successful will bet, policies decided by those with knowledge

## Supply Chain

* Blood diamonds: issues with process to certify ownership of diamond
* Everledger: tokenizes and tracks every diamond
* People can cross-reference diamonds to determine path and location
* Not protected yet against GIGO and corruption

## Smart Energy Grids

* Hard to find a nongovernmental central party to collaborate on infrastructure for citizens
* Make promises through smart contracts to guarantee financial commitment to project
* Immutable commitment scheme to coordinate between untrusting parties
* Citizens can build infrastructure for themselves if they have capital
* Users can be their own service providers

## After-Hours Trading

* Problem: lack of liquidity, increase in volatility and therefore risk
* Tokenize stocks and put marketplace on blockchain
* Broker underwrites stock token: anyone who owns token can exchange for stock
* Stock token can be traded around the world, even after-ours
* Blockchain is accessible: any broker can tap into market

# Blockchain vs Internet

* Both provide protocols for connectivity
* Internet: securely share information with anyone
* Blockchain: connect with anyone to securely but publicly share value
* Information is an information exchange, Blockchain is a value exchange

# Blockchain Generalizations

## Use Cases

* Decentralization must be central to accomplishing some goal
* Efficiency: not efficient to buy a coffee and wait 10 minutes for block confirmation
* Very efficient to send money overseas and not wait for transaction fees
* Blockchain is not the only technology that has fault tolerance/consensus/key cryptography
* Blockchain is over-engineered to these specific problems
* Coordination failures: can create incentive structures, technological solution to social problem

## Horizontal Integration

* Can combine power of all users to enhance capabilities
* Combine data between institutions: all info on blockchain is accessible to everyone
* Enforce common standard: a protocol's specifications
* Network effects: increased value of a produce with additional users
* More smart contract developers benefit the community

## Pure Decentralization

* Decentralization for the sake of keeping something out of control of a central authority
* Useful in countries with distrust in central authorities
* Globally-recognized proofs: cryptocurrencies are guaranteed to be globally accessible

## Benefits of Centralization

* Centralized solutions provide independence: no need for consensus
* Deep integration under the same umbrella to control user experience
* Easier to change individual components or architectures of project
* Efficiency: executing programs can be millions of times less costly, only one store of data needed
* Access controls: simple in a central solution, control read/write permissions
* Complexity: utilization of human judgement, trust a single person to report on state
* Adaptive: human-based error handling, don't need full consensus or agreement of outcome

