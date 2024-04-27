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
    dnsserver.vm.network "private_network", ip: "10.10.10.2", virtualbox__intnet: "h1switch", :netmask => "255.255.255.0"
    dnsserver.vm.provision "shell", inline: <<-SHELL
    sudo ip r add 10.10.20.0/24 via 10.10.10.254
  SHELL
  end

  config.vm.define "client" do |client|
    client.vm.hostname = "client"
    client.vm.network "private_network", ip: "10.10.20.2", virtualbox__intnet: "h2switch", :netmask => "255.255.255.0"
    client.vm.provision "shell", inline: <<-SHELL
    sudo ip r add 10.10.10.0/24 via 10.10.20.254
  SHELL
  end
  
  config.vm.define "client2" do |client2|
    client2.vm.hostname = "client2"
    client2.vm.network "private_network", ip: "10.10.20.3", virtualbox__intnet: "h3switch", :netmask => "255.255.255.0"
    client2.vm.provision "shell", inline: <<-SHELL
    sudo ip r add 10.10.10.0/24 via 10.10.20.254
  SHELL
  end

  config.vm.define "mailserver" do |mailserver|
    mailserver.vm.hostname = "mailserver"
    mailserver.vm.network "private_network", ip: "10.10.10.3", virtualbox__intnet: "h4switch", :netmask => "255.255.255.0"
    mailserver.vm.provision "shell", inline: <<-SHELL
    sudo ip r add 10.10.20.0/24 via 10.10.10.254
  SHELL
  end

  config.vm.define "switchRouter" do |switchRouter|
    switchRouter.vm.box = "ubuntu/jammy64"
    switchRouter.vm.hostname = "switchRouter"
    # Unused ip
    switchRouter.vm.network "private_network", ip: "10.10.30.10", virtualbox__intnet: "h1switch", nic_type: "virtio", :netmask => "255.255.255.0"
    switchRouter.vm.network "private_network", ip: "10.10.30.20", virtualbox__intnet: "h2switch", nic_type: "virtio", :netmask => "255.255.255.0"
    switchRouter.vm.network "private_network", ip: "10.10.30.30", virtualbox__intnet: "h3switch", nic_type: "virtio", :netmask => "255.255.255.0"
    switchRouter.vm.network "private_network", ip: "10.10.30.40", virtualbox__intnet: "h4switch", nic_type: "virtio", :netmask => "255.255.255.0"
    switchRouter.vm.provider "virtualbox" do |virtualbox|
      virtualbox.customize [ "modifyvm", :id, "--nicpromisc2", "allow-vms" ]
      virtualbox.customize [ "modifyvm", :id, "--nicpromisc3", "allow-vms" ]
      virtualbox.customize [ "modifyvm", :id, "--nicpromisc4", "allow-vms" ]
      virtualbox.customize [ "modifyvm", :id, "--nicpromisc5", "allow-vms" ]
    end
  end
end
