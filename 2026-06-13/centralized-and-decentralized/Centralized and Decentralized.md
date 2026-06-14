This is an an attempt at drawing sketches of the another way to architect networks of computers.

# Centralized Model
Currently, we are using a centralized model, where one CPU controls everything. This model was driven by economics of early machines. CPUs and memory were very expensive, so it was not easy to imagine using many machines each with private memory.

This centralized model time-shares the central CPU among many processes and shares memory between the processes. This has led to complications due to issues of "thread safety" and to the illusion of parallelism. In this model, processes do not run in parallel but run sequentially and switch so quickly that humans perceive operations to be happening at the same time, even though they are running sequentially. 

This model of memory sharing does not extend easily to truly distributed, asynchronous systems, like the internet, where nodes are physically distant from one another and run in parallel. This mismatch is being handled with ever more complicated algorithms that need to achieve cache coherency and more complicated hardware architectures like multi-core CPUs with multiple caches and hidden, hardware-level synchronization.

![[centralized-and-decentralized-central.drawio.png]]
# Decentralized Model Based on Messaging

The decentralized model is based on a new 21st century reality: CPUs and memory is inexpensive.

Nodes are physically distant from one another and are connected by relatively narrow bandwidth wires. This model cannot rely on implicit synchronization and low-level memory sharing at the memory cell level.

By adopting a decentralized model, with smart nodes, each with private memory, we can greatly simplify software. We no longer need to worry about thread safety. Our notion of operating systems can change - we no longer need to assume that all software libraries are plugged together in a synchronous, clockwork manner. In fact, we may not need operating systems at all.

Program development systems essentially run on single computers, hence, need to fake out asynchronous message passing an parallelism.
## Smart App
In the decentralized model, programs and software applications essentially own 100% of their own CPU, and are not slowed down by the need to share cycles with other programs.

A thin veneer of software is still needed to receive and send messages using pure dataflow.

![[centralized-and-decentralized-smart-app.drawio.png]]
## Smart Device
In the decentralized model, hardware devices, like printers, are smart and have their own CPU and private memory.

We've already seen this model in production - Apple created smart, postscript-driven printers in contrast with the HP model of baring the hardware guts of printer devices to a central computer.

![[centralized-and-decentralized-smart-device.drawio.png]]
## Decentralized Model

![[centralized-and-decentralized-decentralized.drawio.png]]
## Structured Messaging
Early attempts at building asynchronous message-passing systems failed due to the inability to scale designs.

The answer to this problem is simple - decentralized, message-passing architectures need to be *structured* in some hierarchical fashion. 

We've seen this model in action for decades in successful businesses - ORG charts with strict rules against micro-management and going over the bosses' heads.

In a decentralized computer architecture, no node can communicate directly with its peers. Nodes pass requests down to nodes below them and accept summaries from below. Nodes are arranged in an ORG chart manner - each manager node communicates directly with a only a small number of subordinate nodes. Manager nodes can only communicate directly only with nodes direct below them - nodes cannot reach down deeper and micro-manage.

Nodes cannot communicate directly with their peers. This ensures scalability. Peer to peer communication is entirely managed by middle manager nodes.

### Request Flow

![[centralized-and-decentralized-request-flow.drawio.png]]
### Summary Flow

![[centralized-and-decentralized-summary-flow.drawio.png]]