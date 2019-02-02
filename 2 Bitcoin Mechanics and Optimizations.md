# Cryptographic Hash Functions

## Motivations and Definitions

### Integrity of Information

* Bitcoin: need to design "fingerprints" of meaningful data
* If information is changed, fingerprints change too
* Must not be able to guess the data that formed the fingerprint
* Fingerprints are a form of standardized randomness

### Hash Functions

* Produce a psuedorandom output from a given input
* Appears random to observers, but inputs always produce the same outputs
* Input is pre-image, output is image

## Cryptography

### Preimage Resistance

* Preimage resistance prevents reverse-engineering of hash functions
* Computationally difficult to find the input from the output

### Second Preimage Resistance

* Second preimage resistance prevents stealing of fingerprints
* If someone can generate fingerprint with different inputs, nobody can determine original owner
* Computationally difficult to find an input that matches a separate input's output

### Collision Resistance

* Having two arbitrary inputs mapping to the same output is very bad
* Computationally difficult to find two inputs that form the same output

### Avalanche Effect

* Combination of the above three properties
* Any change in input leads to a pseudorandom change in output
* Prevents getting output based on inputs

### SHA-256

* Hash function that takes arbitrary amount of information and returns 256-bit output
* SHA = Secure Hash Algorithm, 256 the size of output
* Bitcoin uses SHA-256<sup>2</sup>, or performs SHA-256 twice on inputs

# Tamper-Evident Databases

## Bitcoin Blocks

### Dissecting a Block

* Main components: Block Header, Block Size, Transaction Counter, Transactions
* Block Header: stores metadata to understand components of block
* Block Size: tells how large the block is
* Transaction Counter: tells how many transactions the block contains
* Transactions: actual transaction data

### Block Headers

* Enforces blockchain security and tamper evidence, contains hash of all metadata
* Three important fields: Merkle Root, Previous Block Hash, Nonce
* Merkle Root: summary of transactions
* Previous Block Hash: chaining
* Nonce: proof-of-work

## Merkle Root

### Merkle Trees

* Contains all fingerprints of information in one piece of data
* Merkle Root is at the top of a Merkle Tree, a cryptographic data structure
* If any transaction is changed, the transaction's hash and all hashes above it are changed
* Not expensive to verify that a transaction is included

### Merkle Tree Construction

- Merkle Trees are Binary Trees with 2<sup>n</sup> nodes on the bottom level, and the lowest level are hashes of info that needs to be summarized
- Start with transaction, hash each to get a level of hashes, hash pairs together to make a new level, etc.
- The hash at the very top of the tree is a Merkle Tree

## Previous Block Hash

* Contains the hash of the previous block, every block in the blockchain does this
* If any block is altered, all blocks after it will also be altered
* Tampered Merkle root changes header, which changes next block's Previous Block Hash
* The rest of the network will reject the tampered blockchain history

## Nonce

### Proof-of-Work

* Nonce is a physical manifestation of proof-of-work, must design problem for computers to prove their work
* Bitcoin uses a partial preimage hash puzzle: we are given part of input and must find the other part
* Condition: hash of block header must be less than some target value, very common condition

### Hash Problem Characteristics

* Computationally difficult: solution must be hard to find
* Parameterizable: adjustable difficulty, not too easy or hard
* Easily verifiable: not much work needed to check an answer

## Bitcoin Mining

### Block Difficulty

* Mining like throwing darts at a target while blindfolded
* Difficulty: number of expected computations expected to find a block
* Requirement of a leading number of zeros on the block header hash, increases with difficulty
* Global hash rate must raise and lower difficulty based on growth or decay of network
* Difficulty equals itself times the ratio of two weeks to the time taken to mine previous blocks
* Difficulty inversely proportional to time it takes to mine blocks

### Mining Rewards

* Miners make a Coinbase transaction (first transaction of Merkel Tree) when the mine blocks
* Transaction grants miners a reward of bitcoins: this is how bitcoins are minted
* Coinbase transactions have separate nonce fields used in our hash puzzle

### Mining Algorithm

* Loop indefinitely until we find a block: try Coinbase nonce first and then exhaust all header nonce values
* Changing the Coinbase nonce changes the Merkle root, which is more expensive than changing header
* Optimize mining to calculate Merkle root as rarely as possible

# Signatures and Authentication

## Digital Signature Schemes

### Digital Signatures

* Alice must have her own private key to create her signature
* If a message is tampered with, the signature is invalid
* Nobody should be able to guess Alice's private key from her public key

###  Signature Example

* Alice and Bob have each others' public keys and their own private keys
* Alice signs message with private key: generates unique signature
* Sends signature and message to Bob, who validates signature with Alice's public key

### DSS Properties

* Given message, signature, and public key of sender, receiver should be able to verity:
* Message Origin: original sender/owner of private key has authorized this message
* Non-repudiation: original sender/owner of private key cannot backtrack
* Message Integrity: message has not been modified since sending

## Keys and Addresses

### High-Level Overview

* Generate private key randomly, use Elliptic Curve point scalar multiplication to derive public key
* Use hash functions to generate the Bitcoin address
* Elliptic Curves and hashing are one-way processes

### Elliptic Curves

* Bitcoin uses ECDSA/Elliptic Curve Digital Signature Algorithm to produce all keys
* Elliptic Curves are curves defined by the form y<sup>2</sup> = x<sup>3</sup> + ax + b, Bitcoin's is y<sup>2</sup> = x<sup>3</sup> + 7 (secp256k1)
* Taken over a finite field to limit key sizes
* Curves are symmetrical over the x-axis, and can perform addition and multiplication

### Trapdoor Functions

* Trapdoor functions are one-way, cannot get inputs from outputs
* User first generates random number n for private key
* Multiply known generator P by n and hash it twice using different algorithms for public key
* Getting n from nP is the Elliptic Curve Discrete Logarithm Problem, computationally infeasible

### Public Key Hash to Address

* Adds version byte and then runs a human-friendly encoding/hashing system to get address
* Calculates, encodes, and hashes checksum to account for human error

# Bitcoin Script

## Transactions

### Bitcoin and UTXO

* Transactions are outputs from previous transactions feeding to new inputs
* Transactions contain signature of owner of unspent funds: these are UTXOs
* Spending bitcoin necessitates redeeming UTXOs with verification of sender/receiver
* Proof needed is constructed from public key and signature

### Sections of a Transaction

* Metadata: housekeeping data, transaction ID, locktime, size
* Inputs: list of previously-created UTXOs, and proof of identity for UTXO redemption
* Outputs: list of new UTXOs that will be sent to new addresses
* Each value accompanied with script that locks third-parties without proof

### Metadata

* Contains hash/ID of transaction, version, locktime, and size
* vin_sz/vout_sz, vector input/output size, or UTXOs being used/created

### Inputs

* Input hashes are referenced to the unique IDs of previous transactions containing relevant UTXOs
* scriptSig: prove that you can redeem associated UTXOs

### Outputs

* Script locks the transaction and makes it redeemable only with proof
* Written in Satoshis, the smallest unit of Bitcoin (100, 000, 000 to 1)

## Bitcoin Scripts

### Script I/O

* Output addresses are scripts: account can be redeemed by public key that hashes to address X with proof
* Connecting I/O using scripts can make transactions more complex
* Bitcoin Script language is limited in capability for security reasons

### P2PKH

* Redeem a previous transaction output: Bitcoin Pay to public Key Hash (P2PKH)
* Need to prove identification with public key and signature of ownership of private key
* Public key must hash to address to which previous transaction was sent

### Locking and Unlocking Scripts

* Unlocking scripts: provide sig and public key to unlock transaction output, known as "scriptSig"
* Public key is hashed and checked against the address that owns the UTXO
* Locking scripts: requirements for redeeming the UTXO, locking it for all third-parties
* Users must possess a private key that hashes to an address, "scriptPubKey"
* Locking scripts are just concatenated on top of unlocking scripts, and then both are run
* To redeem a UTXO, the UTXO owner provides an unlocking script to some locking script

### P2PKH Example

* Execution order: scriptSig on top, and scriptPubKey on bottom
* scriptSig is input script to new transaction, scriptPubKey is output script of old transaction we are spending from
* Put out signature and public key on the stack
* Execution:
  * Duplicate top item (public key)
  * Hash previous item
  * Compare the hash with public key hash of previous transaction output
  * Checks validity of given signature with given public key

### Proof of Burn

* Can write arbitrary data into Bitcoin blockchain
* If you write a transaction that has OP_RETURN before an output, the resulting output can't be spent
* This leads to the destruction or "burn" of the output
* Below OP_RETURN, you can bootstrap an altcoin, requiring that you burn bitcoin before getting altcoin
* Can write arbitrary data below OP_RETURN, and this data is immutable because the blockchain is

# Advanced Bitcoin Script

## P2SH Specifications

### P2PKH vs P2SH

* P2PKH: vendor/recipient asks customer to send coins to hash of a public key
* Vendor must provide a public key and signature in order to redeem bitcoins
* P2SH: vendor/recipient asks customer to send coins to hash of a script
* Vendor must provide the script as well as the data in order to redeem bitcoins
* Customer-friendly, because the customer does not have to spend time making a Bitcoin script

### P2PKH Hash

* Alice can send transaction to Bob's public key hash
* Bob must provide signature and full public key to redeem the sent Bitcoin
* If Bob wants to introduce functionality into transaction, Bob creates a script, takes hash, and sends it to Alice
* Alice then sends a transaction; Bob must provide signature and full script to redeem

### Why use P2SH?

* Vendor provides hash of script to customer, so vendor writes the script instead of customer
* P2SH: pay to script hash, sender needs only the hash of the script to create locking script
* Customers don't care about the implementation of script or storage 

## Multisignature

### Multisignature Basics

* Specific number of pre-specified signatures needed to unlock a UTXO
* m-of-n multisig scheme: need m of n private keys to unlock the UTXO address
* Increases difficulty of stealing funds, thief must have minimum number of keys n
* Multiple keys to unlock accounts prevents losses, just in case you lose a private key
* Can give control of single address to multiple people

###  Multisig Construction

* Unlocking scripts: contains number of sigs that are enough to satisfy locking script
* Redeeming script: contains all possible sigs to spending the bitcoins, only used when redeemer wants to use money
* Locking script: contains hash that is compared with hash of redeem script

## Timelocks

* Restrict spending of funds until later time or specific block height/number of blocks on top of transaction
* Transaction-level timelocks postpone the transaction itself, transaction cannot be included in valid block
* However, the sender can still send atransaction spending from the same UTXO as timelocked transaction
* UTXO-level timelocks lock the UTXO itself, no override by sending UTXO to another address
* No global time on blockchain: peers' clocks can be off, be careful when using absolute/UNIX timestamps
* Any attempts to cancel out of timelocked transaction should be made hours before the timelock expires

