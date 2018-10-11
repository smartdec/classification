# Classification of smart contract vulnerabilities
Each standard vulnerability is made possible by one of the features of smart contracts. We unite types of vulnerabilities into groups by the corresponding features. We unite several groups into the same class if their features have the same nature.  
Here is the list of classes and groups with some standard vulnerabilities categorized by this classification.  
Many of those vulnerabilities are well described in Smart Contract Weakness Classification [Registry](https://github.com/SmartContractSecurity/SWC-registry).  

## Blockchain
Vulnerabilities caused by blockchain nature of the system.

### Miner creates a block
Miner assembles block and thus can influence its contents (included transactions, their order, other block parameters). 
- front-running / transaction reordering (SWC-114)
- timestamp manipulation (SWC-116)
- random with blockhash (SWC-120)
- transaction censorship

### Contract interaction
Ethereum allows smart contracts to interact with other. The following vulnerabilities are based on the fact that one contract can’t rely on the behaviour of an arbitrary contract.
- unchecked low-level call (SWC-104)
- reentrancy (SWC-107)
- DoS with revert (SWC-113)
- DoS with selfdestruct

### Gas imitations
Blockchains require some payment for a transaction. In Ethereum fee is proportional to the computational complexity of the code being executed. Vulnerabilities of this category are based on the fact that the amount of gas available for the transaction is limited.
- infinite loops
- transfer provides too little gas

### Message structure
Ethereum allows accounts to send messages to each other. This data is converted to function calls according to specific rules. 
- signature collisions
- short address attack

## Language
Vulnerabilities caused by the design of Solidity language (or any other language used for smart contracts).

### Arithmetic
Solidity operates only with integer numbers and doesn’t check the correctness of arithmetic operations.
- over/underflow (SWC-101)
- precision issues

### Storage access
In some cases, state variables can point to an incorrect storage slot.
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
Ethereum was designed as a trustless system. However, many smart contracts are built so that users have to trust the owner/administrator. This part of the system can be compromised or used in an undesirable way.
- overpowered owner
- vulnerable off-chain server

### Privacy
In Ethereum, values of state variables and smart contract bytecode are available for everyone. Some developers use it to store sensitive data.

### Economy
Issues with the economic model of the system.
- voting issues
- tokenomics issues
- Game Theory issues