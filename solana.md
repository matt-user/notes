# Solana

* Tower BFT, PoS
* leaders are known 1 epoch in advance
* PoH - proof of history - maintain time trustlessly
* addresses are pub keys
* programs call eachother through CPI
* PoH ensure timestamps can be trusted
* TBFT - pre-prepare, prepare, commit
* Turbine - block propagation, propagation - prioritized by nodes stake
* gulf stream - mempooless solution for forwarding and storing txs before processing
* sealevel - parallelization layer for processing transactions, each tx describes all states
* TPU - fetch stage: data fetch in kernel space, sig verify using gpu, banking: change state using CPU, broadcast: write to disk
* cloudbreak - db system
* programs - accounts marked executable
* native programs - built directly into core of solana blockchain
* memory - mono heap of data, all state lives on heap
* programs have access to own part of heap
* a memory region is an account
* CPI - one program calling another program

## Pinocchio

* zero copy type - data transfer between memory without cpu copying data
* zero deps
* 