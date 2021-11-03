# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "bento/ubuntu-18.04"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.cpus = 1
    v.linked_clone = true
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end


  # Apache1
  config.vm.define "www1" do |www1|
    www1.vm.hostname = "www1.lab"
    www1.vm.network :private_network, ip: "192.168.2.3"
  end
  
  # Apache2
  config.vm.define "www2" do |www2|
    www2.vm.hostname = "www2.lab"
    www2.vm.network :private_network, ip: "192.168.2.4"
  end
  
  # MySQL1
  config.vm.define "db1" do |db1|
    db1.vm.hostname = "db1.lab"
    db1.vm.network :private_network, ip: "192.168.2.5"
    # db1.vm.network :public_network, bridge: "wlp1s0", ip: "10.51.1.138"
  end

  # Haproxy
  config.vm.define "haproxy" do |haproxy|
    haproxy.vm.hostname = "haproxy.lab"
    # haproxy.vm.network :private_network, ip: "192.168.2.2"
    haproxy.vm.network :public_network, bridge: "wlp1s0", ip: "10.51.1.138"
    # haproxy.vm.network :public_network
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "configure.yml"
    ansible.inventory_path = "inventory/inventory"
    ansible.limit = "all"
  end

end
