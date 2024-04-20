# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/jammy64"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |v|
    v.memory = 512
  end

  config.vm.provision :shell, inline: "sudo apt update && sudo apt upgrade -y"
  
  
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yml"
  end

  config.vm.define "dnsserver" do |dnsserver|
    dnsserver.vm.hostname = "dnsserver"
    dnsserver.vm.network "private_network", ip: "10.10.10.11", virtualbox__intnet: "h1switch"
  end

  config.vm.define "client" do |client|
    client.vm.hostname = "client"
    client.vm.network "private_network", ip: "10.10.10.12", virtualbox__intnet: "h2switch"
  end
  
  config.vm.define "client2" do |client2|
    client2.vm.hostname = "client2"
    client2.vm.network "private_network", ip: "10.10.10.13", virtualbox__intnet: "h3switch"
  end

  config.vm.define "mailserver" do |mailserver|
    mailserver.vm.hostname = "mailserver"
    mailserver.vm.network "private_network", ip: "10.10.10.14", virtualbox__intnet: "h4switch"
  end

  config.vm.define "switch" do |switch|
    switch.vm.box = "ubuntu/jammy64"
    switch.vm.hostname = "switch"
    switch.vm.network "private_network", ip: "10.10.10.1", virtualbox__intnet: "h1switch", nic_type: "virtio"
    switch.vm.network "private_network", ip: "10.10.10.2", virtualbox__intnet: "h2switch", nic_type: "virtio"
    switch.vm.network "private_network", ip: "10.10.10.3", virtualbox__intnet: "h3switch", nic_type: "virtio"
    switch.vm.network "private_network", ip: "10.10.10.4", virtualbox__intnet: "h4switch", nic_type: "virtio"
    switch.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y openvswitch-switch
        ovs-vsctl add-br br0
        ovs-vsctl add-port br0 enp0s8
        ovs-vsctl add-port br0 enp0s9
        ovs-vsctl add-port br0 enp0s10
        ovs-vsctl add-port br0 enp0s16
        ovs-vsctl set port enp0s9 tag=1000
        ovs-vsctl set port enp0s10 tag=1000
        ovs-vsctl set port enp0s8 tag=2000
        ovs-vsctl set port enp0s16 tag=2000
    SHELL
    switch.vm.provider "virtualbox" do |virtualbox|
      virtualbox.customize [ "modifyvm", :id, "--nicpromisc2", "allow-vms" ]
      virtualbox.customize [ "modifyvm", :id, "--nicpromisc3", "allow-vms" ]
      virtualbox.customize [ "modifyvm", :id, "--nicpromisc4", "allow-vms" ]
      virtualbox.customize [ "modifyvm", :id, "--nicpromisc5", "allow-vms" ]
    end
  end
end
