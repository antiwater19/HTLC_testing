# Introduction
An HTLC is a conditional transfer of value from "depositor" to "recipient" where two distinct conditions prevent immediate execution. 1) The hashlock requires presenting the proper "secret" to the blockchain prior to the 2) timelock expiration, else the value automatically returns to "depositor".

This allows two parties to exchange assets on independent platforms trustlessly and securely and thus enables Atomic Cross-Chain Swaps (ACCS) among other useful functionalities.

# hashed-timelock-contract-lc-ethereum
This is to demonstrate an atomic swap of two blockchain networks using HTLC. <br/>
Two participants: Peter and Han<br/>
Two blockchain networks:  Learning Coin (LC) and Ethereum Ropsten Testnet (ETH)<br/>
Transaction: Peter will send 1 LC to Han's LC wallet, in exchange, Han will send 2 ETH to Peter's ETH wallet. 

[Hashed Timelock Contracts](https://en.bitcoin.it/wiki/Hashed_Timelock_Contracts) (HTLCs) for Ethereum:

- [HashedTimelock.sol](contracts/HashedTimelock.sol) - HTLC for native tokens exchange

Use these contracts for creating HTLCs on the Ethereum side of a cross chain atomic swap (for example the [xcat](https://github.com/chatch/xcat) project).

# Process flow
## Creation Phase: 
having agreed on the parameters, Peter and Han create the HTLCs.

```
  1. Peter creates an HTLC on LC network with the receiver as Han. 1 LC is locked into the LC's HTLC. 
     He shares the smart contract ID and Hash(the hashlock).
  2. Han waits for the HTLC to confirm on the LC blockchain and validates all parameters, including 
     but not limited to, that she is the receiver, that the amount is appropriate, and the HTLC 
     hash-lock matches the agreed upon Hash.
  3. Han creates an HTLC on Ethereum with the receiver as Peter. 2 ETH are locked in the Ethereum HTLC.
  4. Peter waits for the HTLC to confirm on the Ethereum blockchain and validates that he is the 
     receiver and the HTLC hash-lock matches the agreed upon Hash.
```

## Redemption Phase: 
with the HTLCs created on both blockchains, Peter and Han can now proceed to the settlement.

```
  1. Peter redeems the ETH on Ethereum by redeeming the HTLC by submitting the Secret(pre-image) in
     a transaction. In doing so, he exposes the Secret(pre-Image).
  2. Han waits for Peter to expose the Secret, and then Han redeems the HTLC on LC using the same Secret.
```

## Done: 
the swap between Peter and Han is now complete.

## Run Tests
* Install truffle
* Install ganache [https://truffleframework.com/ganache](https://truffleframework.com/ganache)
* Launch and set the network ID to `4447`

```
$ npm i
$ truffle test
Using network 'test'.

Compiling ./test/helper/ASEANToken.sol...
Compiling ./test/helper/EUToken.sol...


  Contract: HashedTimelock
    ✓ newContract() should create new contract and store correct details (92ms)
    ✓ newContract() should fail when no ETH sent (84ms)
    ✓ newContract() should fail with timelocks in the past (78ms)
    ✓ newContract() should reject a duplicate contract request (159ms)
    ✓ withdraw() should send receiver funds when given the correct secret preimage (214ms)
    ✓ withdraw() should fail if preimage does not hash to hashX (111ms)
    ✓ withdraw() should fail if caller is not the receiver (162ms)
    ✓ withdraw() should fail after timelock expiry (1243ms)
    ✓ refund() should pass after timelock expiry (1273ms)
    ✓ refund() should fail before the timelock expiry (132ms)
    ✓ getContract() returns empty record when contract doesn't exist (48ms)

  Contract: HashedTimelockERC20
    ✓ newContract() should create new contract and store correct details (214ms)
    ✓ newContract() should fail when no token transfer approved (107ms)
    ✓ newContract() should fail when token amount is 0 (166ms)
    ✓ newContract() should fail when tokens approved for some random account (214ms)
    ✓ newContract() should fail when the timelock is in the past (136ms)
    ✓ newContract() should reject a duplicate contract request (282ms)
    ✓ withdraw() should send receiver funds when given the correct secret preimage (363ms)
    ✓ withdraw() should fail if preimage does not hash to hashX (227ms)
  ...
```

## Protocol - Native token exchange between Learning Coin (LC) and Ethereum (ETH)

### Main flow

![](docs/sequence-diagram-htlc-lc-eth-success.png?raw=true)

### Timelock expires

![](docs/sequence-diagram-htlc-eth-refund.png?raw=true)


## Interface

### HashedTimelock

1.  `newContract(receiverAddress, hashlock, timelock)` create new HTLC with given receiver, hashlock and expiry; returns contractId bytes32
2.  `withdraw(contractId, preimage)` claim funds revealing the preimage
3.  `refund(contractId)` if withdraw was not called the contract creator can get a refund by calling this some time after the time lock has expired.

See [test/htlc.js](test/htlc.js) for examples of interacting with the contract from javascript.
