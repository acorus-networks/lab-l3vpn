# LAB L3VPN - By Acorus Networks

# Requirement

This Vagrantfile will spawn 3 instances of VQFX (Full), 3 customers and 1 server for management

### Resources
 - RAM : 10G
 - CPU : 6 Cores

# Topology
```
                                +---------+
                                |         |
                           +----+   SRV   +------+
                           |    |         |      |
                           |    +----+----+      |
                           |         |           |
                           |         |           |
                           |         |           |
                       xe-0/0/4      |       xe-0/0/4
+---------+           +---------+    |     +---------+          +---------+
|         |   xe-0/0/2|         | xe-0/0/0 |         |xe-0/0/2  |         |
|  CUST1  +-----------+  VQFX1  +----+-----+  VQFX2  +----------+  CUST2  |
|         |           |         |    |     |         |          |         |
+---------+           +--+------+    |     +-----+---+          +---------+
                xe-0/0/1 |           |           |  xe-0/0/1
                         |        xe-0/0/4       |
                         |      +---------+      |
                         |      |         |      |
                         +------+  VQFX3  +------+
                       xe-0/0/1 |         | xe-0/0/0
                                +---------+
                                     |xe-0/0/2
                                     |
                                     |
                                     |
                                +----+----+
                                |         |
                                |  CUST3  |
                                |         |
                                +---------+
```

# Provisioning

All VQFX and server will be preconfigured.


- vqfx: Ansible is used to configure interfaces
- cust: Customer
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

```
acorus@demo01:~$ cd lab-l3vpn
acorus@demo01:~/lab-l3vpn$ vagrant status
```

#### SSH to devices on lab :

Connnect to your POD demo server.
Then SSH to each device in the lab, repeat for vqfx1, vqfx2, vqfx3, srv, cust1, etc.

```
acorus@demo01:~$ cd lab-l3vpn
acorus@demo01:~/lab-l3vpn$ vagrant ssh vqfx1
```

#### Test on VQFX1 :

Run below command on vqfx1 :

```
vagrant@vqfx1> show configuration
```

OUTPUT

```
vagrant@vqfx1> show configuration
## Last commit: 2019-01-24 12:34:12 UTC by root
version 17.4R1.16;
system {
    host-name vqfx1;
    root-authentication {
        encrypted-password
...
```

#### Test on SRV :

Run below command on the srv :

```
vagrant@server:~$ ifconfig
vagrant@server:~$ ping 192.168.100.20
vagrant@server:~$ ssh vqfx1
```
You should be able to SSH lab VQFX from srv (192.168.100.10) under noc user :

OUTPUT
 
```
vagrant@server:~$ ssh vqfx1
--- JUNOS 17.4R1.16 built 2017-12-19 20:03:37 UTC
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

TASK [Print IP of remote device] *********************************************************************************************************************
ok: [vqfx1] => {
    "msg": "192.168.100.20"
}
ok: [vqfx2] => {
    "msg": "192.168.100.21"
}
ok: [vqfx3] => {
    "msg": "192.168.100.22"
}

TASK [Retrieving information from devices running Junos OS] ******************************************************************************************
ok: [vqfx2]
ok: [vqfx3]
ok: [vqfx1]

TASK [Print version] *********************************************************************************************************************************
ok: [vqfx1] => {
    "junos.version": "17.4R1.16"
}
ok: [vqfx2] => {
    "junos.version": "17.4R1.16"
}
ok: [vqfx3] => {
    "junos.version": "17.4R1.16"
}

PLAY RECAP *******************************************************************************************************************************************
vqfx1                      : ok=4    changed=0    unreachable=0    failed=0
vqfx2                      : ok=4    changed=0    unreachable=0    failed=0
vqfx3                      : ok=4    changed=0    unreachable=0    failed=0
```
Everything is tested and ready, let's start the lab now !


# Labs


## OSPF

```
ansible-playbook -i inventories/hosts pb.test.igp.yaml --vault-id ~/.vault_pass.txt
```

## iBGP
```
ansible-playbook -i inventories/hosts pb.test.bgp.yaml --vault-id ~/.vault_pass.txt
```

## L3VPN
```
ansible-playbook -i inventories/hosts pb.test.l3vpn.yaml --vault-id ~/.vault_pass.txt
```