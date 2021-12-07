
# Packet Reflector - P4 Utils
## 📑 Introduction
1. What is P4 programming language?
* The programming language P4 is gaining in popularity in the network industry
* P4 has emerged as the standard language for describing how network packets should be processed
2. What is P4 Utils?
* P4-Utils is an extension to Mininet that makes P4 networks easier to build, run and debug
* P4-utils is strongly inspired by p4app
3. P4 Utils project - Packet Reflector
* Creating simple topologies with hosts and p4 switches 
* Adding links between them
* Making switches bouncing back packets to the interface the packets came from





## ⚙️ Installation

In order to work, P4-Utils needs different programs coming from different sources as prerequisites
* virtual machine configured to work with P4-Utils
* manual installation using an installation script.
### Build own virtual machine
To get started, install the required software
* VirtualBox
* Packer

Then, clone the P4-Utils repository:
```bash
git clone https://github.com/nsg-ethz/p4-utils
```
Go to the Packer configurations folder:
```bash
cd p4-utils/vm
```
To build the VirtualBox VM, execute:
```bash
./build-virtualbox.sh [--cpus 4] [--disk_size 25000] [--memory 4000] [--vm_name p4] [--username p4] [--password p4]
```
### Install P4-utils
After configuring the VirtualBox VM,  install P4-Utils using the following commands:
```bash
git clone https://github.com/nsg-ethz/p4-utils
cd p4-utils
sudo ./install.sh
```
## 🔎 Project details
### First Step
Some preconfigured files P4-Utils repository provides that help to complete the project:
* ``` p4app.json ```: describes the topology we want to create with the help of Mininet and the P4-Utils package
* ``` network.py ```: a Python scripts that initializes the topology using Mininet and P4-Utils.
* ``` send_receive.py ```: script to send and receive packets.
* ``` reflector.p4 ```: p4 program skeleton to use as a starting point.

To create a mininet network and run ``` reflector.p4 ```, do the following steps:
1. To create the topology described in ``` p4app.json ```, call ```p4run```:

```bash
sudo p4run
```
After running the network with one of the two aforementioned methods, we will get the mininet CLI prompt:

![alt text](https://github.com/nsg-ethz/p4-learning/raw/master/exercises/01-Reflector/images/mininet_cli.png)

2. At this point we will have a small topology that consists of a host h1 and a p4 switch s1. We can get a terminal in h1 by either typing xterm h1 in the CLI, or by using the mx command
```bash
xterm h1
```
3. From the h1 terminal we can run ``` send_receive.py ```, a small python script that sends packets to the switch and prints if the packets get reflected.

4. Close all the host-terminals and type ``` quit ``` to leave the mininet CLI and clean the network.

### Implementing the Packet Reflector
In the ```reflector.p4``` skeleton, we should do the following:
1. Parse the ``` ethernet ``` header and make a transition to ``` MyIngress ```
2. Swap the packet's ethernet addresses
3. Use the ``` ingress_port ``` as ``` egress_port ```. The value of the ```ingress_port``` will be stored in the packet metadata, in the variable ``` standard_metadata.ingress_port ```. To set a packet's output port, we need to set ``` standard_metadata.egress_spec ``` metadata field.
4. Deparse the ``` ethernet ``` header.

### Testing the solution
When we finished the 4 steps above we can repeat the steps explained in First Steps section. This time when we send a packet we should get a reflected packet from the switch with the MAC addresses swapped:
![alt text](https://drive.google.com/drive/u/0/folders/1a01MrTTUKCco6jRWGbv7gomuEYaxsCDe)

## Reference
https://github.com/nsg-ethz/p4-learning/tree/master/exercises/01-Reflector
