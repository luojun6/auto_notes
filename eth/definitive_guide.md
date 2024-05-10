# The Definitive Guide

## Automotive Etherneet (100BASE-T1/1000BASE-T1)

Automotive ethernet or normal ethnert is not a bus technology, it's findamentally different than a bus technology.

Ethernet in all modern forms currently available is essentially just a point-to-point network. If you want multiple nodes more than two to communicate to each other. We have to introduce what's call **switch** into the equation and the **switch** enables multiple ECU's to each other, and a switch routes traffic to differnet nodes with a network based on their physical address that's the primary. Primary use of a switch butin the current forms of automoative ehternet.

- Pros

  - Up to 1000 Mb/s (each direction and each leg)
  - Widely used technology (much support)
  - Good clock synchronization technology available (based on IEEE 1588)
  - History of adapation to solve new problems

- Cons

  - Requires a switch
  - Not possible to add or remove nodes unless the switch has spare ports
  - Tools cannot just connect and sniff the bus

![auto_eth_example_ivi](./images/auto_eth_example_ivi.png)
