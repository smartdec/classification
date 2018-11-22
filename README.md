# Classification of smart contract vulnerabilities
Each standard vulnerability is made possible by one of the features of smart contracts. We unite types of vulnerabilities into groups by the corresponding features. We unite several groups into the same class if their features have the same nature.
Here is the list of classes and groups with some standard vulnerabilities categorized by this classification.

## Blockchain
Vulnerabilities caused by blockchain nature of the system.

### Block content manipulation
Miner assembles block and thus can influence its contents (included transactions, their order, other block parameters).
- Front-running / transaction reordering ([SWC-114](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-114), [DASP-7](https://dasp.co/#item-7), [SP-10](https://blog.sigmaprime.io/solidity-security.html#SP-10))
- Timestamp manipulation ([SWC-116](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-116), [DASP-8](https://dasp.co/#item-8), [SP-12](https://blog.sigmaprime.io/solidity-security.html#SP-12))
- Random with blockhash ([SWC-120](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-120), [DASP-6](https://dasp.co/#item-6), [SP-6](https://blog.sigmaprime.io/solidity-security.html#SP-6))
- Transaction censorship ([link](https://blog.ethereum.org/2015/06/06/the-problem-of-censorship/))

### Contract interaction
Ethereum allows smart contracts to interact with each other. The following vulnerabilities are based on the fact that one contract cannot rely on the behaviour of an arbitrary contract.
- Unchecked low-level call ([SWC-104](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-104), [DASP-4](https://dasp.co/#item-4), [SP-9](https://blog.sigmaprime.io/solidity-security.html#SP-9))
- Reentrancy ([SWC-107](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-107), [DASP-1](https://dasp.co/#item-1), [SP-1](https://blog.sigmaprime.io/solidity-security.html#SP-1))
- DoS with revert ([SWC-113](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-113), [SP-11](https://blog.sigmaprime.io/solidity-security.html#SP-11))
- DoS with selfdestruct ([DASP-5](https://dasp.co/#item-5))

### Gas limitations
Blockchains require some payment for a transaction execution. In Ethereum, fee is proportional to the computational complexity of the code being executed. Vulnerabilities of this category are based on the fact that the amount of gas available for the transaction is limited.
- Infinite loops ([SWC-129](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-129))
- Transfer provides too little gas: receiving contract has only 2300 gas for its fallback function, which is not enough for external call, writing to storage, etc

### Message structure
Ethereum allows accounts to send messages to each other. This data is converted to function calls according to specific rules.
- Signature collisions: two different functions may have the same signature
- Short address attack ([DASP-9](https://dasp.co/#item-9), [SP-8](https://blog.sigmaprime.io/solidity-security.html#SP-8))

### Ether transfer
Generally, all ETH transfers invoke contract’s fallback function and, thus, can be detected by the contract. However, there are several ways of ETH transfer that cannot be detected by the contract. Mostly, they are described in ([SP-3](https://blog.sigmaprime.io/solidity-security.html#SP-3)).
- Ether transfer with selfdestruct: contract cannot handle incoming Ether sent by `selfdestruct` function
- Ether transfer with mining: contract cannot handle incoming Ether sent as a reward for block mining
- Pre-sent Ether: contract cannot handle Ether sent to its address prior to the deploy

## Language
Vulnerabilities caused by the insecure use of Solidity language (or any other language used for smart contracts).

### Arithmetic
Solidity operates only with integer numbers and does not check the correctness of arithmetic operations.
- Over/underflow ([SWC-101](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-101), [DASP-3](https://dasp.co/#item-3), [SP-2](https://blog.sigmaprime.io/solidity-security.html#SP-2))
- Precision issues ([SP-15](https://blog.sigmaprime.io/solidity-security.html#SP-15))

### Storage access
In some cases, state variables can point to an incorrect storage slot.
- Uninitialized storage pointer ([SWC-109](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-109), [SP-14](https://blog.sigmaprime.io/solidity-security.html#SP-14))
- Delegatecall and storage layout ([SWC-112](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-112), [SP-4](https://blog.sigmaprime.io/solidity-security.html#SP-4))
- Overlap attack ([SWC-124](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-124))

### Internal control flow
Some features of Solidity can lead to overcomplicated control flow graph.
- Multiple inheritance ([SWC-125](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-125))
- Arbitrary jump with function type variable ([SWC-127](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-127))
- Assembly return in constructor: this trick tampers with standard deployment process; as a result, actually deployed bytecode has little in common with the source code

## Model
Vulnerabilities caused by mistakes in the model (architecture) of the system.

### Authorization
Vulnerabilities connected with the insufficient or incorrect authorization implementation ([DASP-2](https://dasp.co/#item-2)).
- Generous contracts ([SWC-105](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-105))
- Suicidal contracts ([SWC-106](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-106))
- Authorization with tx.origin ([SWC-115](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-115), [SP-16](https://blog.sigmaprime.io/solidity-security.html#SP-16))
- Constructor name ([SWC-118](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-118), [SP-13](https://blog.sigmaprime.io/solidity-security.html#SP-13))

### Trust
Ethereum was designed as a trustless system. However, many smart contracts are built so that users have to trust the owner/administrator. This part of the system can be compromised or used in an undesirable way.
- Overpowered owner ([SP-11](https://blog.sigmaprime.io/solidity-security.html#SP-11) - see 3. Owner operations)
- Vulnerable off-chain server: giving too much power to back-end can lead to undesired consequences if the off-chain server is hacked

### Privacy
In Ethereum, smart contracts bytecode and values of state variables are available for everyone. However, some developers use it to store sensitive data.

### Economy
Issues with the economic model of the system.
- Voting issues: wrong voting logic that can lead to undesired effects
- Tokenomics issues: undesired users behaviour caused by economic conjuncture of the token
- Game Theory issues: undesired users behaviour caused by their feasible benefits

## References

The following registries were addressed in the classification.

1. Smart Contract Weakness Classification [Registry](https://smartcontractsecurity.github.io/SWC-registry/).
2. DASP [TOP-10](https://dasp.co/index.html).
3. Sigma Prime [Blog](https://blog.sigmaprime.io/solidity-security.html).