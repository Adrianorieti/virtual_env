Vagrant.configure("2") do |config|

  config.vm.box = "debian/bullseye64"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
  end

  config.vm.provision :shell, inline: "sudo apt update && sudo apt upgrade -y"
  
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yml"
  end

  config.vm.define "dnsserver" do |dnsserver|
    dnsserver.vm.hostname = "dnsserver"
    dnsserver.vm.network "private_network", ip: "192.168.56.12"
  end

  config.vm.define "client" do |client|
    client.vm.hostname = "client"
    client.vm.network "private_network", ip: "192.168.56.11"
  end
  
  config.vm.define "client2" do |client2|
    client2.vm.hostname = "client2"
    client2.vm.network "private_network", ip: "192.168.56.13"
  end

  config.vm.define "mailserver" do |mailserver|
    mailserver.vm.hostname = "mailserver"
    mailserver.vm.network "private_network", ip: "192.168.56.10"
  end


end
