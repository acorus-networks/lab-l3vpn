# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

UUID = "OGVIFL"

### LINKs and Segment
# seg1: vqfx1:xe-0/0/0 -> seg1
# seg2: vqfx1:xe-0/0/1 -> seg2
# seg3: vqfx1:xe-0/0/2 -> seg3
# seg4: vqfx1:xe-0/0/3
# seg5: vqfx1:xe-0/0/4 -> seg61
##
# seg11: vqfx2:xe-0/0/0 -> seg1
# seg12: vqfx2:xe-0/0/1 -> seg12
# seg13: vqfx2:xe-0/0/2 -> seg13
# seg14: vqfx2:xe-0/0/3
# seg15: vqfx2:xe-0/0/4 -> seg61
##
# seg21: vqfx3:xe-0/0/0 -> seg12
# seg22: vqfx3:xe-0/0/1 -> seg2
# seg23: vqfx3:xe-0/0/2 -> seg23
# seg24: vqfx3:xe-0/0/3
# seg25: vqfx3:xe-0/0/4 -> seg61
##
# seg31: cust1:eth0 -> seg61
# seg32: cust1:eth1 -> seg3
## 
# seg41: cust2:eth0 -> seg61
# seg42: cust2:eth1 -> seg13
##
# seg51: cust3:eth0 -> seg61
# seg52: cust3:eth1 -> seg23
##
# seg61: srv:eth0 -> seg61


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|


    config.ssh.insert_key = false

    ### VQFX1 : 
    ###########
    config.vm.define "pfe1" do |vqfxpfe|
        vqfxpfe.ssh.insert_key = false
        vqfxpfe.vm.box = 'juniper/vqfx10k-pfe'
        vqfxpfe.vm.boot_timeout = 1200

        # DO NOT REMOVE / NO VMtools installed
        vqfxpfe.vm.synced_folder '.', '/vagrant', disabled: true
        vqfxpfe.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_vqfx_internal_1"

        # In case you have limited resources, you can limit the CPU used per vqfx-pfe VM, usually 50% is good
        # vqfxpfe.vm.provider "virtualbox" do |v|
        #    v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
        # end
    end

    config.vm.define "vqfx1" do |vqfx|
        vqfx.vm.hostname = "vqfx1"
        vqfx.vm.box = 'juniper/vqfx10k-re'
        vqfx.vm.boot_timeout = 1200

        # DO NOT REMOVE / NO VMtools installed
        vqfx.vm.synced_folder '.', '/vagrant', disabled: true

        # Management port
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_vqfx_internal_1"
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_reserved-bridge"

        # Dataplane ports
        # xe-0/0/0
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg1"
        # xe-0/0/1
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg2"
        # xe-0/0/2
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg3"
        # xe-0/0/3
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg4"
        # xe-0/0/4
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg61"
    end

    ### VQFX2 : 
    ###########
    config.vm.define "pfe2" do |vqfxpfe|
        vqfxpfe.ssh.insert_key = false
        vqfxpfe.vm.box = 'juniper/vqfx10k-pfe'
        vqfxpfe.vm.boot_timeout = 1200

        # DO NOT REMOVE / NO VMtools installed
        vqfxpfe.vm.synced_folder '.', '/vagrant', disabled: true
        vqfxpfe.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_vqfx_internal_2"

        # In case you have limited resources, you can limit the CPU used per vqfx-pfe VM, usually 50% is good
        # vqfxpfe.vm.provider "virtualbox" do |v|
        #    v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
        # end
    end

    config.vm.define "vqfx2" do |vqfx|
        vqfx.vm.hostname = "vqfx2"
        vqfx.vm.box = 'juniper/vqfx10k-re'
        vqfx.vm.boot_timeout = 1200

        # DO NOT REMOVE / NO VMtools installed
        vqfx.vm.synced_folder '.', '/vagrant', disabled: true

        # Management port
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_vqfx_internal_2"
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_reserved-bridge"

        # Dataplane ports
        # xe-0/0/0
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg1"
        # xe-0/0/1
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg12"
        # xe-0/0/2
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg13"
        # xe-0/0/3
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg14"
        # xe-0/0/4
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg61"
    end

    ### VQFX3 : 
    ###########
    config.vm.define "pfe3" do |vqfxpfe|
        vqfxpfe.ssh.insert_key = false
        vqfxpfe.vm.box = 'juniper/vqfx10k-pfe'
        vqfxpfe.vm.boot_timeout = 1200

        # DO NOT REMOVE / NO VMtools installed
        vqfxpfe.vm.synced_folder '.', '/vagrant', disabled: true
        vqfxpfe.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_vqfx_internal_2"

        # In case you have limited resources, you can limit the CPU used per vqfx-pfe VM, usually 50% is good
        # vqfxpfe.vm.provider "virtualbox" do |v|
        #    v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
        # end
    end

    config.vm.define "vqfx3" do |vqfx|
        vqfx.vm.hostname = "vqfx3"
        vqfx.vm.box = 'juniper/vqfx10k-re'
        vqfx.vm.boot_timeout = 1200

        # DO NOT REMOVE / NO VMtools installed
        vqfx.vm.synced_folder '.', '/vagrant', disabled: true

        # Management port
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_vqfx_internal_2"
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_reserved-bridge"

        # Dataplane ports
        # xe-0/0/0
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg12"
        # xe-0/0/1
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg2"
        # xe-0/0/2
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg33"
        # xe-0/0/3
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg34"
        # xe-0/0/4
        vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "#{UUID}_seg61"
    end

    ##########################
    ## Server          #######
    ##########################
    config.vm.define "srv" do |srv|
        srv.vm.box = "ubuntu/xenial64"
        srv.vm.hostname = "server"
        # eth0 
        srv.vm.network 'private_network', ip: "192.168.100.10", virtualbox__intnet: "#{UUID}_seg61"
        srv.vm.boot_timeout = 1200
        srv.ssh.insert_key = true
    end

    ### Cust1 : 
    ###########
    config.vm.define "cust1" do |cust|
        cust.vm.box = "debian/buster64"
        cust.vm.hostname = "cust1"
        # eth1
        cust.vm.network 'private_network', ip: "192.168.100.11", virtualbox__intnet: "#{UUID}_seg61"
        # eth2
        cust.vm.network 'private_network', ip: "100.64.1.1/31", virtualbox__intnet: "#{UUID}_seg3"
        cust.vm.boot_timeout = 1200
        cust.ssh.insert_key = true
    end

    ### Cust2 : 
    ###########
    config.vm.define "cust2" do |cust|
        cust.vm.box = "debian/buster64"
        cust.vm.hostname = "cust2"
        # eth1
        cust.vm.network 'private_network', ip: "192.168.100.12", virtualbox__intnet: "#{UUID}_seg61"
        # eth2
        cust.vm.network 'private_network', ip: "100.64.2.2", virtualbox__intnet: "#{UUID}_seg13"
        cust.vm.boot_timeout = 1200
        cust.ssh.insert_key = true
    end

    ### Cust3 : 
    ###########
    config.vm.define "cust3" do |cust|
        cust.vm.box = "debian/buster64"
        cust.vm.hostname = "cust3"
        # eth1 
        cust.vm.network 'private_network', ip: "192.168.100.13", virtualbox__intnet: "#{UUID}_seg61"
        # eth2
        cust.vm.network 'private_network', ip: "100.64.3.2", virtualbox__intnet: "#{UUID}_seg23"
        cust.vm.boot_timeout = 1200
        cust.ssh.insert_key = true
    end



    ##############################
    ## VQFX provisioning       ###
    ## exclude Windows host    ###
    ##############################
    if !Vagrant::Util::Platform.windows?
        config.vm.provision "ansible" do |ansible|
            ansible.groups = {
                "vqfx10k" => ["vqfx1", "vqfx2", "vqfx3"],
                "vqfx10k-pfe" => ["vqfx1-pfe", "vqfx2-pfe", "vqfx3-pfe"],
                "all:children" => ["vqfx10k", "vqfx10k-pfe"]
            }
            ansible.playbook = "provisioning/deploy-config.p.yaml"
        end
    end

    ##############################
    ## Server provisioning     ###
    ## exclude Windows host    ###
    ##############################
    if !Vagrant::Util::Platform.windows?
        config.vm.provision "ansible" do |ansible|
            ansible.groups = {
                "server" => ["srv"]
            }
            ansible.playbook = "provisioning/deploy-srv.yaml"
        end
    end

    ##############################
    ## Server provisioning     ###
    ## exclude Cust host    ###
    ##############################
    if !Vagrant::Util::Platform.windows?
        config.vm.provision "ansible" do |ansible|
            ansible.groups = {
                "cust" => ["cust1", "cust2", "cust3"]
            }
            ansible.playbook = "provisioning/deploy-cust.yaml"
        end
    end
end
