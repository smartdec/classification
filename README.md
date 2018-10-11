# Classification of smart contract vulnerabilities
Some features of the Ethereum ecosystem lead to standard vulnerabilities. We categorize those vulnerabilities by corresponding feature. We place several groups of vulnerabilities to the same class if their features have same nature.  
Here is list of classes and their groups with some standard vulnerabilities categorized by such classification.  
Many of those vulnerailities are well described in Smart Contract Weakness Classification [Registry](https://github.com/SmartContractSecurity/SWC-registry).  

## Blockchain
Vulnerabilities caused by blockchain nature of the system.

### Miner creates block
Miner assembles block and thus can influence its contents (included transactions, their order, other block parameters). 
- front-running / transaction reordering (SWC-114)
- timestamp manipulation (SWC-116)
- random with blockhash (SWC-120)
- transaction censorship

### Contract interaction
Ethereum allows smart contracts to interact with others. These vulnerabilities are based on the fact that one contract can’t rely on behaviour of arbitrary contract.
- unchecked low-level call (SWC-104)
- reentrancy (SWC-107)
- DoS with revert (SWC-113)
- DoS with selfdestruct

### Gas imitations
Blockchains require some payment for a transaction. In Ethereum fee is proportional to the computation complexity required for code execution. Vulnerabilities of this category are based on the fact that the amount of gas available for the transaction is limited.
- infinite loops
- transfer provides too little gas

### Message structure
Ethereum allows accounts to send messages to each other. This data is converted to function calls according to specific rules. 
- signature collisions
- short address attack

## Language
Vulnerabilities caused by design of Solidity language (or any other language used for smart contracts).

### Arithmetic
Solidity operates only with integer numbers and doesn’t check correctness of arithmetic operations.
- over/underflow (SWC-101)
- precision issues

### Storage access
In some cases state variables can point to incorrect storage slot.
- uninitialized storage pointer (SWC-109)
- delegatecall and storage layout (SWC-112)
- overlap attack

### Internal Control Flow
Some features of Solidity can lead to overcomplicated control flow graph.
- multiple inheritance (SWC-125)
- arbitrary jump with function type variable (SWC-127)
- assembly return in constructor

## Model
Vulnerabilities caused by mistakes in the model (architecture) of the system.

### Authorization
Vulnerabilities connected with the insufficient or incorrect authorization implementation.
- generous contracts (SWC-105)
- suicidal contracts (SWC-106)
- authorization with tx.origin (SWC-115)
- constructor name (SWC-118)

### Trust
Ethereum was designed as trustless system. However, many smart contracts are designed so that users have to trust the owner. This part of the system can become compromised and/or used in an undesirable for users way.
- overpowered owner
- vulnerable off-chain server

### Privacy
In Ethereum, values of state variables and smart contracts bytecode are available for everyone. Some developers use it to store sensitive data.

### Economy
Issues with Game Theory, Tokenomics etc.
