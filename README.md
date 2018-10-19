# Classification of smart contract vulnerabilities
Each standard vulnerability is made possible by one of the features of smart contracts. We unite types of vulnerabilities into groups by the corresponding features. We unite several groups into the same class if their features have the same nature.  
Here is the list of classes and groups with some standard vulnerabilities categorized by this classification.

##References
The following registries were addressed in the classification.
1. Smart Contract Weakness Classification [Registry](https://github.com/SmartContractSecurity/SWC-registry). 
2. DASP [TOP-10](https://dasp.co/index.html).
3. Sigma Prime [Repository](https://github.com/sigp/solidity-security-blog).

## Blockchain
Vulnerabilities caused by blockchain nature of the system.

### Block content manipulation
Miner assembles block and thus can influence its contents (included transactions, their order, other block parameters).
- Front-running / transaction reordering ([SWC-114](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-114.md), [DASP-7](https://dasp.co/#item-7), [Race Conditions / Front Running](https://github.com/sigp/solidity-security-blog#race-conditions--front-running-1))
- Timestamp manipulation ([SWC-116](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-116.md), [DASP-8](https://dasp.co/#item-8), [Block Timestamp Manipulation](https://github.com/sigp/solidity-security-blog#block-timestamp-manipulation-1))
- Random with blockhash ([SWC-120](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-120.md), [DASP-6](https://dasp.co/#item-6), [Entropy Illusion](https://github.com/sigp/solidity-security-blog#entropy-illusion-1))
- Transaction censorship ([link](https://blog.ethereum.org/2015/06/06/the-problem-of-censorship/))

### Contract interaction
Ethereum allows smart contracts to interact with each other. The following vulnerabilities are based on the fact that one contract cannot rely on the behaviour of an arbitrary contract.
- Unchecked low-level call ([SWC-104](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-104.md), [DASP-4](https://dasp.co/#item-4), [Unchecked CALL Return Values](https://github.com/sigp/solidity-security-blog#unchecked-call-return-values-1))
- Reentrancy ([SWC-107](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-107.md), [DASP-1](https://dasp.co/#item-1), [Re-Entrancy](https://github.com/sigp/solidity-security-blog#re-entrancy))
- DoS with revert ([SWC-113](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-113.md), [Denial Of Service (DOS)](https://github.com/sigp/solidity-security-blog#denial-of-service-dos-1))
- DoS with selfdestruct ([DASP-5](https://dasp.co/#item-5))

### Gas limitations
Blockchains require some payment for a transaction execution. In Ethereum, fee is proportional to the computational complexity of the code being executed. Vulnerabilities of this category are based on the fact that the amount of gas available for the transaction is limited.
- Infinite loops: the execution of a function might exceed `block.gaslimit` due to a costly operations executed in a loop
- Transfer provides too little gas: receiving contract has only 2300 gas for its fallback function, which is not enough for external call, writing to storage, etc

### Message structure
Ethereum allows accounts to send messages to each other. This data is converted to function calls according to specific rules.
- Signature collisions: two different functions may have the same signature
- Short address attack ([DASP-9](https://dasp.co/#item-9), [Short Address/Parameter Attack](https://github.com/sigp/solidity-security-blog#short-addressparameter-attack-1))

### Ether transfer
Generally, all ETH transfers invoke contract’s fallback function and, thus, can be detected by the contract. However, there are several ways of ETH transfer that cannot be detected by the contract. Mostly, they are described in ([Unexpected Ether](https://github.com/sigp/solidity-security-blog#unexpected-ether-1)).
- Ether transfer with selfdestruct: contract cannot handle incoming Ether sent by selfdestruct function
- Ether transfer with mining: contract cannot handle incoming Ether sent as a reward for block mining
- Pre-sent Ether: contract cannot handle Ether sent to its address prior to the deploy

## Language
Vulnerabilities caused by the insecure use of Solidity language (or any other language used for smart contracts).

### Arithmetic
Solidity operates only with integer numbers and does not check the correctness of arithmetic operations.
- Over/underflow ([SWC-101](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-101.md), [DASP-3](https://dasp.co/#item-3), [Arithmetic Over/Under Flows](https://github.com/sigp/solidity-security-blog#arithmetic-overunder-flows))
- Precision issues ([Floating Points and Precision](https://github.com/sigp/solidity-security-blog#floating-points-and-precision))

### Storage access
In some cases, state variables can point to an incorrect storage slot.
- Uninitialized storage pointer ([SWC-109](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-109.md))
- Delegatecall and storage layout ([SWC-112](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-112.md), [Delegatecall](https://github.com/sigp/solidity-security-blog#delegatecall-1))
- Overlap attack ([SWC-124](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-124.md))

### Internal Control Flow
Some features of Solidity can lead to overcomplicated control flow graph.
- Multiple inheritance ([SWC-125](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-125.md))
- Arbitrary jump with function type variable ([SWC-127](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-127.md))
- Assembly return in constructor: this trick tampers with standard deployment process; as result actually deployed bytecode has little common with the source code

## Model
Vulnerabilities caused by mistakes in the model (architecture) of the system.

### Authorization
Vulnerabilities connected with the insufficient or incorrect authorization implementation ([DASP-2](https://dasp.co/#item-2)).
- Generous contracts ([SWC-105](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-105.md))
- Suicidal contracts ([SWC-106](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-106.md))
- Authorization with tx.origin ([SWC-115](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-115.md), [Tx.Origin Authentication](https://github.com/sigp/solidity-security-blog#txorigin-authentication-1))
- Constructor name ([SWC-118](https://github.com/SmartContractSecurity/SWC-registry/blob/master/entries/SWC-118.md), [Constructors with Care](https://github.com/sigp/solidity-security-blog#constructors-with-care-))

### Trust
Ethereum was designed as a trustless system. However, many smart contracts are built so that users have to trust the owner/administrator. This part of the system can be compromised or used in an undesirable way.
- Overpowered owner ([Owner operations](https://github.com/sigp/solidity-security-blog#dos))
- Vulnerable off-chain server: giving too much power to back-end can lead to undesired consequences if the off-chain server is hacked

### Privacy
In Ethereum, smart contracts bytecode and values of state variables are available for everyone. However, some developers use it to store sensitive data.

### Economy
Issues with the economic model of the system.
- Voting issues: wrong voting logic that can lead to undesired effects
- Tokenomics issues: undesired users behaviour justified by economic conjuncture of the token
- Game Theory issues: undesired users behaviour justified by their feasible benefits