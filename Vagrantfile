# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "geerlingguy/centos7"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.username = 'vagrant'
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.cpus = 1
    v.linked_clone = true
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # Varnish
  config.vm.define "varnish" do |varnish|
    varnish.vm.hostname = "varnish.lab"
    varnish.vm.network :private_network, ip: "192.168.2.2"
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
  end

  # MySQL2
  config.vm.define "db2" do |db2|
    db2.vm.hostname = "db2.lab"
    db2.vm.network :private_network, ip: "192.168.2.6"
  end

  # Memcached
  config.vm.define "memcached" do |memcached|
    memcached.vm.hostname = "memcached.test"
    memcached.vm.network :private_network, ip: "192.168.2.7"

    # Run Ansible provisioner once for all VMs at the end.
    memcached.vm.provision "ansible" do |ansible|
      ansible.playbook = "configure.yml"
      ansible.inventory_path = "inventory/inventory"
      ansible.limit = "all"
      ansible.extra_vars = {
        ansible_ssh_user: 'vagrant',
        ansible_ssh_private_key_file: "~/.vagrant.d/insecure_private_key"
      }
    end
  end
  # Grafana metrics
  # config.vm.define "metrics" do |metrics|
  #   metrics.vm.hostname = "metrics.lab"
  #   metrics.vm.network :private_network, ip: "192.168.2.8"
  # end
end
