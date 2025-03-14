# Learn about Validators

### Introduction[​](https://docs.astar.network/docs/build/nodes/collator/learn#introduction) <a href="#introduction" id="introduction"></a>

A validator plays an essential role in our network and is responsible for crucial tasks, including block production and transaction confirmation. A validator needs to maintain a high communication response capability to ensure the seamless operation of the Uomi ecosystem.

### Role of validators in the Uomi ecosystem[​](https://docs.astar.network/docs/build/nodes/collator/learn#role-of-collators-in-the-astar-ecosystem) <a href="#role-of-collators-in-the-astar-ecosystem" id="role-of-collators-in-the-astar-ecosystem"></a>

Validators maintain our ecosystem by collecting transactions from users and validating blocks securing the network.

Performance of the network depends directly on validators. To ensure optimal performance of the network, a slashing mechanism is implemented.

#### UOMI-ENGINE[​](https://docs.astar.network/docs/build/nodes/collator/learn#xcmp) <a href="#xcmp" id="xcmp"></a>

Validators are a key element of UOMI-ENGINE, they executes Agents when a request is made by an user, and save the output for the OPoC

***

### Validator election mechanism[​](https://docs.astar.network/docs/build/nodes/collator/learn#collator-election-mechanism) <a href="#collator-election-mechanism" id="collator-election-mechanism"></a>

#### Election process[​](https://docs.astar.network/docs/build/nodes/collator/learn#election-process) <a href="#election-process" id="election-process"></a>

To join the election process you must register for a validator and bond tokens, see Validator Requirements for details. When your node fits the parameters and checks all the boxes to become a validator, it will be added to the chain. **Note: if your validator doesn’t produce blocks during two sessions (2h) it will be kicked out.**

***

### Validator reward distribution mechanism[​](https://docs.astar.network/docs/build/nodes/collator/learn#collator-reward-distribution-mechanism) <a href="#collator-reward-distribution-mechanism" id="collator-reward-distribution-mechanism"></a>

At every block you produced as a validator, rewards will automatically be transferred to your account. The reward includes block reward + fees + Agents fees.

***

### Slash mechanism[​](https://docs.astar.network/docs/build/nodes/collator/learn#slash-mechanism) <a href="#slash-mechanism" id="slash-mechanism"></a>

A slashing mechanism is implemented on Uomi and Finney networks - a validator that doesn't produce blocks during two sessions (2 hours) will be slashed 1% of its total stake and kicked out of the active validator set. This slashing ensures the best block rate and prevents malicious actors from harming the network without financial consequences.
