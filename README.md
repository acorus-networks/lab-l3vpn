# LAB L3VPN - By Acorus Networks

# Requirement

This Vagrantfile will spawn 4 instances of VQFX (Full), 5 customers and 1 server for management

### Resources
 - RAM : 16 G
 - CPU : 6 Cores

# Topology
```
                                +----------+
                                |  Server  |
                                +----------+


                    +---------+              +---------+
                    |  CUST1  |              |  CUST2  |
                    +----+----+              +----+----+
                         |                        |
                         | xe-0/0/2               |
                         |                        |xe-0/0/2
                     +---+---+                +-------+
                     |       |    xe-0/0/0    |       |
                     | vQFX1 +----------------+ vQFX2 |
                     |       |                |       |
                     +---+---+                +---+---+
                         |   |                    |
                         |   |    xe-0/0/3        |
                xe-0/0/1 |   +----------------+   | xe-0/0/1
                         |                    |   |
                         |                    |   |
                     +---+---+                +---+---+
+---------+ xe-0/0/3 |       |                |       |
|  CUST5  +----------+ vQFX4 +----------------+ vQFX3 |
+---------+          |       |    xe-0/0/0    |       |
                     +---+---+                +---+---+
                         |                        |
                         | xe-0/0/2               | xe-0/0/2
                         |                        |
                    +----+----+               +---+-----+
                    |  CUST4  |               |  CUST3  |
                    +---------+               +---------+
```

IPv4 address used:
- Loopback vQFX: 10.200.0.{id}
- Loopback cust: 10.100.0.{id}
- IGP: 10.0.0.X
- Interco core/cust: 100.64.{id_cust}.0/31

IPv4 address used:
- Loopback vQFX: fd00::{id}
- Loopback cust: fddd::{id}
- IGP: fd01::X
- Interco core/cust: fd02:{id_cust}::0/127

# Provisioning

All VQFX and server will be preconfigured.

- vqfx: Ansible is used to configure interfaces
- cust: Customer (with 2 BGP sessions ipv4/ipv6)
- srv: Ansible is used to prepare the machine (packages, ansible and demo files)

## Tools

- Ansible 
- Vagrant / Virtualbox
- Ansible Vault

## Lab setup

#### SSH on the demo machine :

```
$ ssh acorus@{{your_pod_ip}}
```

#### Check vagrant env (shoud be ready to use) :

Check all devices are virtualized
```
acorus@instance-mpls:~$ cd lab-l3vpn
acorus@instance-mpls:~/lab-l3vpn$ vagrant status
```

#### SSH to devices on lab :

Connnect to your demo server.
Then SSH to each device in the lab

```
acorus@instance-mpls:~$ cd lab-l3vpn
acorus@instance-mpls:~/lab-l3vpn$ vagrant ssh vqfx1
```

```
acorus@instance-mpls:~/lab-l3vpn$ vagrant ssh vqfx1
--- JUNOS 19.4R1.10 built 2019-12-19 03:54:05 UTC
{master:0}
vagrant@vqfx1>
```

#### Test on SRV :

Run below command on the srv :

```
vagrant@server:~$ ping 192.168.100.21
vagrant@server:~$ ssh noc@192.168.100.21
--- JUNOS 19.4R1.10 built 2019-12-19 03:54:05 UTC
{master:0}
noc@vqfx1>
```

Let's try with Ansible now :

```
vagrant@server$ ansible-playbook -i inventories/hosts pb.test.netconf.yaml --vault-id ~/.vault_pass.txt
```

OUTPUT

```
vagrant@server:~$ ansible-playbook -i inventories/hosts pb.test.netconf.yaml --vault-id ~/.vault_pass.txt

PLAY [Compare  Junos OS] *****************************************************************************************************************************

TASK [Checking NETCONF connectivity] *****************************************************************************************************************
ok: [vqfx2]
ok: [vqfx3]
ok: [vqfx1]

TASK [Print IP of remote device] **************************************************************************************************
ok: [vqfx2] => {
    "msg": "192.168.100.21"
}
ok: [vqfx1] => {
    "msg": "192.168.100.20"
}
ok: [vqfx3] => {
    "msg": "192.168.100.22"
}

TASK [Retrieving information from devices running Junos OS] ***********************************************************************
ok: [vqfx3]
ok: [vqfx2]
ok: [vqfx1]

TASK [Print version] **************************************************************************************************************
ok: [vqfx3] => {
    "junos.version": "19.4R1.10"
}
ok: [vqfx1] => {
    "junos.version": "19.4R1.10"
}
ok: [vqfx2] => {
    "junos.version": "19.4R1.10"
}

PLAY RECAP ************************************************************************************************************************
vqfx1                      : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
vqfx2                      : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
vqfx3                      : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Everything is tested and ready, let's start the lab now !

# Labs


## OSPF
Configure all IGP settings:
- Backbone interfaces
- OSPF
- MPLS

```
ansible-playbook -i inventories/hosts pb.juniper.igp.yaml --vault-id ~/.vault_pass.txt
```

## iBGP
Configure all iBGP sessions (full mesh)
- BGP

```
ansible-playbook -i inventories/hosts pb.juniper.ibgp.yaml --vault-id ~/.vault_pass.txt
```

## L3VPN
Configure all customers L3VPN
- VRF L3VPN
- Customer interfaces
- Customer sessions

```
ansible-playbook -i inventories/hosts pb.juniper.l3vpn.yaml --vault-id ~/.vault_pass.txt
```

Can check on the customer
```
vagrant ssh cust1
ip route
ping 5.5.5.5
ping6 fddd::5
```