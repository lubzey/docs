# Бяла книга за NeoContract

## Предговор

Смарт контрактите се отнасят до всякаква компютърна програма, която може автоматично да изпълни условията на своя предварително програмиран контракт. Идеята за смарт контракт е била предложена за пръв път от криптографа Ник Забо (Nick Szabo) през 1994, което я прави стара колкото и самият интернет. Поради липсата на надеждна среда за изпълнение, смарт контрактите не са били широко използвани.

През 2008, човек на име Сатоши Накамото пусна Bitcoin и очерта основните концепции на блокчейна. В блокчейна на Bitcoin Накамото използва набор от скриптови езици, за да помогне на потребителите да придобият повече гъвкавост при контрола на личните си акаунти и процеса на трансфер, което в крайна сметка стана ембрионна форма на верижно-базирана смарт контракт система.

През 2014 г. тийнейджърът Виталик Бутерин (Vitalik Buterin), пусна Ethereum, която предоставя верижно-базирана, Touring-завършена, смарт контракт система,която може да бъде използвана  за създаване на различни децентрализирани блокчейн приложения.

NEO блокчейн е платформа за дигитални активи и приложения, която предоставя нова смарт контракт система, NeoContract. В сърцевината на платформата NEO, мрежата предоставя множество функции като възможности за дигитални активи, NeoAsset, и дигитална идентичност, NeoID, което позволява на потребителите лесно да се включат в дигиталния бизнес, и вече не се ограничава само до издаването на нейтив тоукъни на блокчейна.

Тази статия представя характеристиките на NeoContract и изследва нетехнически подробности. Моля, вижте техническата документация за техническите подробности: [docs.neo.org](http://docs.neo.org).

## Характеристики

### Сигурност

Ако дадена програма се изпълнява на различни компютри или по различно време на същия компютър, поведението на програмата е детерминистичен, и ако тя гарантира че с едни и същи входни данни гарантира ще произведе същите изходни данни, както и обратното.

Блокчейн е едновременно метод за многоточково съхранение и изчислителен метод, при който данните в рамките на тази разпределена система са резултат от надеждни изчисления, които не могат да бъдат манипулирани. Смарт контрактите действат в рамките на много-ноудовата, разпределена блокчейн мрежа. Ако смарт контрактът не е детерминистичен, резултатите от различните ноудове може да са несъвместими. В резултат на това не може да се постигне консенсус между ноудовете, което блокира мрежата. Следователно, при проектирането на смарт контракт система е необходимо да се изключат всички фактори, които могат да доведат до недетерминистично поведение.

#### Време

Получаването на системно време е много често срещана системна функция, която може да има голямо приложение в определени процедури, свързани с времето. Обаче получаването на системно време е недетерминистичена системна функция и е трудно да се получи унифицирано точно време в дистрибутирана система, тъй като резултатите от различните ноудове ще бъдат несъвместими. NeoContract предоставя системно повикване базирано на блокове, което третира целия блокчейн като сървър за времеви отпечатък и го получава, когато се генерира нов блок. Мрежата на NEO генерира нов блок на всеки 15 секунди, така че контрактът работи приблизително по същото време като последния блок-време, плюс-минус 15 секунди.

#### Случайност

Много смарт контракт програми, като например контракти за хазарт и малки игри, използват функции с произволни числа. Функциите с случайни числа обаче са типична недетерминистична функция и всяко повикване на системата ще получи различни резултати. В дистрибутирана система има много начини за решаване на този проблем: Първо, същите произволни семена могат да бъдат използвани за всички възли, така че последователността на връщане на цялата произволна функция е детерминирана, но този метод излага предварително целия случайен резултат предварително, като значително намалява практическата стойност на случайното число. Друго възможно решение е всички ноудове да комуникират съвместно, за да генерират произволни номера. Това може да бъде постигнато чрез използване на криптографски техники за получаване на справедливо случайно число, но недостатъкът е в много ниската производителност и необходимостта от допълнителна комуникация. Централизираният доставчик на случаен номер може да се използва за генериране на произволни номера, които гарантират последователност и производителност, но недостатъкът на този подход е очевиден: потребителите ще трябва безусловно да се доверят на централизиран доставчика на номера.

Има два начина за генериране на случаен номер в NEO:

1. Когато се генерира всеки блок, консенсусният ноуд ще постигне консенсус на случайно число и ще го запълни в полето Nonce на новия блок. Програмата за контракта може лесно да получи произволния номер на всеки блок, като посочи полето Nonce.

2. Програмата на контракта може да използва хеширата стойност на блок като генератор за случаен номер, защото хешираната стойност има определена наследена случайност. Този метод може да се използва за придобиване на слаб случаен номер.

#### Източник на информация

Ако дадена програма получи данни по време на изпълнение, тя може да стане недетерминистична програма, ако източникът на данни предоставя недетерминистични данни. Например, използването на различни търсачки за получаване на първите 10 резултата от търсене за дадена ключова дума може да доведе до различни резултати в различни поръчки за сортиране, ако се използват различни IP адреси.

NEO предоставя два вида детерминистични източници на данни за смарт контракти:

- **Блокчейн леджър**
  Процедурата на контракта има достъп до всички данни за целия блокчейн чрез оперативно съвместими услуги, включително завършени блокове и транзакции, както и всяко от техните полета. Данните на блоковете са детерминистични и последователни, така че те могат да бъдат достъпени по сигурен начин чрез смарт контракти.

- **Пространство за съхренение на информация на контракт**

   Всеки договор, доставен в мрежата на NEO, има собствено пространство за съхранение на информация, до което има достъп само контракта. Консенсусният механизъм на NEO осигурява консистентност на състоянието на съхранените данни на всеки ноуд в мрежата.

За ситуации, при които е необходим достъп до данни извън блокчейна, NEO не предоставя пряк начин за взаимодействие с такива данни. Данни, които не са на блокчейна, трябва да бъдат прехвърлени към блокчейна на NEO, използвайки транзакции, а в последствие преведени в някой от горепосочените източници на данни, за да станат достъпни чрез смарт контракти.

#### Contract Call

Smart contracts in NeoContract can call each other, but not be recursively called. Recursion can be achieved within the contract, but it cannot cross the boundaries of the current contract. In addition, the call relationship between contracts must be static: The target cannot be specified at runtime. This allows the behavior of the program to be fully determined before execution, and its call relationship to be fully defined before it can run. Based on this, multiple contracts can be dynamically partitioned to achieve parallel execution.

### High Performance

The execution environment of a smart contract plays an integral role in its performance. When we analyze the performance of any execution environment, there are main two indicators which are critical:

1. Execution speed of the instruction
2. Startup speed of the execution environment itself

For smart contracts, the execution environment is often more important than the speed of execution of the instruction. Smart contracts are more involved in IO operation of the logic, to determine the instructions, where the implementation of these instructions can easily be optimized. Every time the smart contract is called, it must start up a new virtual machine / container. Therefore, the execution speed of the environment itself (starting a virtual machine / container) has greater impact on the performance of the smart contract system. 

NEO uses a lightweight NeoVM (NEO Virtual Machine) as its smart contract execution environment, which has a very fast start up and takes up very little resources, perfect for short programs like smart contracts. Using the compilation and caching of hotspot smart contracts with JIT (real-time compiler) can significantly improve the efficiency of virtual machines.

### Scalability

#### High concurrency and dynamic partitioning

When discussing the scalability of a system, it involves two main areas: Vertical scaling and horizontal scaling. Vertical scaling refers to the optimization of the processing workflow, allowing the system to take full advantage of existing equipment capacity. With this approach, limits of the system are easily reached, as series-based processing capacity is based on the hardware limit of a single device. When we need to scale the system, is there a way to transform the series system into a parallel system? Theoretically, we will only need to increase the number of devices, and we will be able to achieve almost unlimited scalability. Could we possibly achieve unlimited scaling in distributed blockchain networks? In other words, can the blockchain execute programs in parallel?

The blockchain is a distributed ledger, that records a variety of state data and the rules governing the changes in state of these data. Smart contracts are used as carriers, to record these rules. Blockchains can process programs in parallel, only if, multiple smart contracts can be executed concurrently and in a non-sequential manner. Basically, if contracts do not interact with each other, or if the contract does not modify the same state data, at the same time, their execution is non-sequential and can be executed concurrently. Otherwise, it can only execute in series, following a sequential order, and the network is unable to scale horizontally.

Based on the analysis above, we can easily design "unlimited scaling" in smart contract systems. All we must do is to set up simple rules:

 * **A smart contract can only modify the state record of the contract that it belongs to**

 * **In the same transaction batch (block), a contract can only be running once**

As a result, all the smart contracts can be processed in parallel as sequential order is irrelevant to the result. However, if a "smart contract can only modify the state record of the contract that it belongs to", it implies that the contract cannot call each other. Each contract, is an isolated island; if "In the same transaction batch (block), a contract can only be running once", this implies that a digital asset issued with a smart contract, can only handle one transaction per block. This is a world of difference with the original design goals of "smart" contracts, which cease to be "smart". After all, our design goals include both mutual call between contracts, and multiple execution of the same call, in the same block.

Fortunately, smart contracts in NEO have a static call relationship, and the call target cannot be specified at run time. This allows the behavior of the program to be fully determined before execution, and its call relationship to be fully defined before it can run. We require that each contract explicitly indicate the contracts which are likely to be invoked, so that the operating environment can calculate the complete call tree before running the contract procedure, and partition execution of the contracts, based on the call tree. Contracts that may modify the same state record, are executed in a sequential manner within the same partition, whereby different partitions can then be executed in parallel.

#### Low coupling

Coupling is a measure of the dependency between two or more entities. NeoContract system uses a low-coupling design, which is executed in the NEOVM, and communicates with the non-blockchain data through the interoperable service layer. As a result, most upgrades to smart contract functions can be achieved by increasing the API of interoperable services.

## Contract Use

### Verification Contract

Unlike the public-key account system used in Bitcoin, NEO's account system uses the contract account system. Each account in the NEO corresponds to a verification contract, and the hash value of the verification contract, is the account address; The program logic of the verification contract controls the ownership of the account. When transferring from an account, you firstly need to execute the verification contract for that account. A validation contract can accept a set of parameters (usually a digital signature or other criteria), and return a boolean value after verification, indicating the success of the verification to the system.

The user can deploy the verification contract to the blockchain beforehand, or publish the contract content directly in the transaction during the transfer process.

### Application Contract

The application contract is triggered by a special transaction, which can access and modify the global state of the system, and the private state of the contract (storage area) at run time. For example, you can create a global digital asset in a contract, vote, save data, and even dynamically create a new contract, when the contract is running.

The execution of the application contract requires charging by instruction. When the transaction fee is consumed, the contract will fail and stop execution, and all state changes will be rolled back. The success of the contract does not affect the validity of the transaction.

### Function Contract

The function contract is used to provide some public or commonly used functions, which can be called by other contracts. The smart contract code can be reused, so that developers are able to write increasingly complex business logic. Each function contract, when deployed, can choose to have a private storage area that is either read or written to data in a future contract, achieving state persistence.

The function contract must be pre-deployed to the chain to be invoked, and removed from the chain by a "self-destructing" system function, which will no longer be used and its private storage will be destroyed. The old contract data can be automatically migrated to another subcontract before it is destroyed, using contract migration tools.

## Virtual Machine

### Virtual Hardware

NEOVM provides a virtual hardware layer, to support the execution of smart contracts, including:

 * **CPU**

   CPU is responsible for reading and sequentially order the execution of instructions in the contract, according to the function of the instruction flow control, arithmetic operations, logic operations. The future of the CPU function can be extended, with the introduction of JIT (real-time compiler) function, thereby enhancing the efficiency instruction execution. 

 * **Call Stack**

   The call stack is used to hold the context information of the program execution at each function call, so that it can continue to execute in the current context after the function has finished executing and returning.

 * **Calculate Stack**

   All NEOVM run-time data are stored in the calculation stack, when after the implementation of different instructions, the stack will be calculated on the corresponding data elements of the operation. For example, when additional instructions are executed, the two operations participating in the addition are ejected from the calculation stack, and the result of the addition is pushed to the top of the stack. Function call parameters must also be calculated from right to left, according to the order of the stack. After the function is successfully executed, the top of the stack fetch-function returns the value.

 * **Spare Stack**

  When you need to schedule or rearrange elements in the stack, you can temporarily store the elements in the spare stack and retrieve them in the future.

### Instruction Set

NEOVM provides a set of simple, and practical instructions for building smart contract programs. According to functions, the main categories are as follows:

 * Constant instruction
 * Process control instruction
 * Stack operation instruction
 * String instruction
 * Logic instruction
 * Arithmetic operation instruction
 * Cryptographic instruction
 * Data operation instruction

It is worth noting that the NeoVM instruction set provides a series of cryptographic instructions, such as ECDSA, SHA and other algorithms to optimize the implementation efficiency of cryptographic algorithms in smart contracts. In addition, data manipulation instructions directly support arrays and complex data structures.

### Interoperable Service Layer

The virtual machine where smart contract executes is a sandbox environment, that requires an interoperable service layer, in times when it needs to access data outside of the sandbox or to keep the run-time data persistent. Within the interoperable service layer, NEO contract can open a series of system function and services with the smart contract program, and these contracts can be called and accessed, like ordinary functions. All system functions are being conducted concurrently, so there is no need to worry about scalability.

### Debugging Function	

Often, the development of smart contracts is very difficult, due to the lack of good debugging and testing methods. NeoVM provides program debugging support at the virtual machine level, where you can set the breakpoint on the contract code, or single-step, single-process execution. Thanks to the low coupling design between the virtual machine and the blockchain, it is easy to integrate NeoVM directly with various IDEs, to provide a test environment that is consistent with the final production environment.

## High-level Languages

### C#, VB.Net, F#

Developers can use NeoContract for almost any high-level language they are good at. The first batch of supported languages ​​are C #, VB.Net, F #, etc. We provide compilers and plug-ins for these languages, ​​allowing compilation of these high-level language into the instruction set, supported by NeoVM. As the compiler focus on MSIL (Microsoft intermediate language) during compilation, so theoretically, any. Net language can be translated into MSIL language, and become directly supported.

A huge number of developers are proficient in these languages, and the above languages have a very strong integrated development environment. Developers can develop, generate, test and debug, all within Visual Studio, where they are able to take full advantage of the smart contract development templates we provide, to gain a head start.

### Java, Kotlin

Java and Kotlin ​​forms the second batch of supported languages, where we provide compilers and IDE plugins for these languages, ​​to help developers use the JVM-based language to develop NEO's Smart Contract applications.

Java is widely used, and Kotlin has recently become the official Google recommended, Android-development language. We believe that supporting these languages will help drastically increase the number of NEO smart contract developers.

### Other Languages

Afterwards, NeoContract will add support for other high-level languages, based on the degree of difficulty, in the complier development process. Some of the languages that may be supported, include:

 * C, C++, GO
 * Python, JavaScript

In the future, we will continue to add more high-level language support. Our goal is to see more than 90% of NEO developers developing with NeoContract, without needing to learn a new language, and even possibly transfer existing business system code directly onto the blockchain.

## Service	

### Blockchain Ledger

NEO Smart Contracts can obtain complete block data for the NEO blockchain, including complete blocks and transactions, and each of their fields, at runtime, through the system functions provided by the interoperable service. Specifically, you can query these data:

 * Height of the blockchain
 * Block head, current block
 * Transactions
 * Type of transaction, attributes, input, output, etc.

Through these data, you can develop some interesting applications, such as automatic payouts, smart contracts based upon proof of workload.

### Digital Assets

Through the interoperable services provided by the digital asset interface, smart contracts not only can query the NEO blockchain on properties and statistics of various digital assets, but also, create new digital assets during its run-time. Digital assets created by smart contracts can be issued, transferred, traded outside of the contract. They are the same as original assets on NEO, and can be managed with any NEO-compatible, wallet software. This specific interface includes:

 * Asset attribute inquiry
 * Asset statistics query
 * Asset life cycle management: create, modify, destroy, etc.
 * Asset management: multi-language name, total change, precision change, changes in the administrator

### Persistence

Each smart contract program deployed on the NEO blockchain, will have a private storage area that can only be read and written by the contract itself. Smart contracts have full operational permissions on the data in its own store: can be read, written, modified, deleted. The data is stored in the form of key-value pairs and provides these interfaces:

 * Traverse all the records stored
 * Return to a specific record according to the specified key
 * Modify or write new records according to the specified key
 * Delete the record according to the specified key

In general, a contract can only read and write data from its own store, with one exception: when a contract is invoked, the invoked contract can access the caller's store through a cross-domain request, provided that the caller provides authorization. In addition, for a sub-contract that is dynamically created at the time of contract execution, the parent contract gets instant access to its store.

Cross-domain requests enable NeoContract to implement rich library capabilities, that provide highly scalable data management capabilities for the callers.

## Fees

### Deployment Fee

NEO's distributed architecture provides high redundancy of storage capacity, and the use of this capacity is not free. Deploying a smart contract on the NEO network requires a fee, currently fixed at 500GAS, which is collected by the system and recorded as a system gain. Future fees will be adjusted according to the actual operating cost in the system. The smart contract deployed on the blockchain can be used multiple times, until the contract is destroyed by the deployer.

### Implementation Fee

NEO provides a credible execution environment for smart contracts, and the execution of contracts requires the consumption of computing resources for each node, therefore users are required to pay for the execution of smart contracts. The fee is determined by the computational resources consumed with each execution, and the unit price is also in GAS. If the implementation of the smart contract fails due to lack of GAS, the cost of consumption will not be returned, and this prevents malicious attacks on the network power consumption.

For most simple contracts, they can be executed for free, so long as the execution costs remain under 10 GAS, thus greatly reducing costs for the user. 

## Application Scenarios

### Superconducting Transactions

Digital assets on the blockchain inherently require some form of liquidity and usually point-to-point transactions cannot provide it sufficiently.  Therefore, there is a need for exchanges to provide users with trading services. Digital asset exchanges can be broadly divided into two categories:

1. **Central exchanges:** where the user needs to deposit the digital assets with the exchange, and subsequent place pending orders for trading, on the website
2. **Decentralized exchanges:** where its trading system is built into the blockchain, and the system provides the matching services.

Centralized exchanges can provide very high performance and diversified services, but need to have a strong credit guarantee, otherwise there will be moral hazards; such as misappropriation of user funds, fraud, etc. Comparatively, decentralized exchange has no moral hazard, but the user experience is poor, and there is greater performance bottleneck. Is there a way to combine both solutions and achieve the best of both worlds?

Superconducting transactions is a mechanism that can do this; Users do not need to deposit assets, where they are able to use their own assets on the blockchain in trading. Transaction settlement complete on the blockchain, but the process of matching orders occurs off-chain, by a central exchange that provides matching services. Since the matching is conducted off-chain, its efficiency is like centralized exchanges, but the assets remain under the control of the user. Exchanges uses the user's trading intent to carry out matching services, with no moral hazards involved, such as misappropriation of user funds, fraud, etc.

At present, within the NEO community, development of smart contracts to achieve blockchain superconducting transactions has emerged, such as OTCGO.

### Smart Fund

Smart funds based on the blockchain have the benefit of being decentralized, open and transparent, possessing a higher degree of security and freedom as compared to traditional funds. These smart funds are also cross-border and open to investors worldwide, where outstanding projects can be funded with capital from all across the world.

Nest is a NeoContract-based smart fund project, which is very similar to the TheDAO project based on Ethereum, where improved security measures is needed to avoid the mistakes of TheDAO (hackers).

### Cross-chain Interoperability

In the foreseeable future, there will be many public chains and thousands of alliance chains or private chains in existence worldwide. These isolated blockchain systems are islands of value and information, which are not interoperable with each other. Through the cross-chain interoperability mechanism, numerous isolated blockchains can be linked, so that the values in different blockchains can be exchanged with each other, to achieve the true value of the Internet.

NeoContract provides support for the implementation of cross-chain interoperability, ensuring consistency within cross-chain asset exchange, cross-chain distributed transactions, and execution of smart contracts on different blockchains.

### Oracle Machines

The concept of oracles in folktale lies in the ability of a certain supernatural entity that can answer a particular set of questions. In the blockchain, oracle machines open the door to the outside world for smart contracts, making it possible for smart contracts to use real-world information as a condition for contract execution.

NeoContract does not provide the ability to directly access external data, such as access to resources on the Internet, because this will introduce non-deterministic behavior, resulting in inconsistencies between nodes during contract execution. Implementing the oracle machine in NeoContract requires that external data be sent to the blockchain by a trusted third party, integrating these data feeds as part of the blockchain ledger, thereby eliminating non-determinism.

The credible third party mentioned above, may be a person or institution that is co-trusted by both parties in the contract, or a decentralized data provider that is guaranteed by economic incentives. In this manner, NeoContract can be used in the implementation of such Oracle machines.

### Ethereum DAPP

Bitcoin created the era of blockchains and electronic cash, and Ethereum created the era of smart contracts. Ethereum, the pioneers of smart contract on the blockchain, has made great contributions to the design idea, economic model and technological realization of a smart contract system. At the same time, the Ethereum platform has seen many DAPPs (distributed applications), where functionalities including: gambling agreements, digital assets, electronic gold, gaming platform, medical insurance, marriage platform, with widespread use over many industries. In theory, all of these DAPPs can be easily transplanted onto the NeoContract platform, as a NEO application.
